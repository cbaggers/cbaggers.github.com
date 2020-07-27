---
layout: post
title: TaleSpire Dev Log 208
description:
date: 2020-07-28 01:08:09
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today, working with a total star from the community, we've been able to track down one cause of the "Stuck a main menu with a spinning hourglass bug".

> TLDR: Check your proxy settings. You may find that manual proxy is enabled, but the address is blank. This exposes a bug in Mono (a thing Unity relies on), which breaks the code that finds the TaleSpire servers.
>
> Here is what the setting looks like when it's wrong. We think this might have been set by a Windows update, but we are not sure.
>
> NOTE: This will not fix cases where the spinning hourglass is only sometimes a problem. I would love to look into any hourglass related issue you have though, so please reach out to me (@Baggers) on the TaleSpire discord.

![the problem](/assets/images/proxyIssue0.png)

Now for the extended version of the story.

Many backers have never been able to play the TaleSpire beta due to a bizarre bug that means they get stuck waiting for TaleSpire to log in forever. Today I was once again struggling with this issue, so I decided to make a program to test the connection and make logs that would hopefully help us progress. I wanted the code to be as close to TaleSpire's as possible, so I took the whole game and spent several hours ripping out pieces until I got down to just the essentials for the main menu. I added some logging and ended up with this:

![TsConnectionTest](/assets/videos/TsConnectionTest.gif)

With a new weapon in hand, I teamed up with a community member who had experienced the issue. For the next hour, they ran the test app and send me the logs. I would then modify the app and send them a new build. Together we finally got this trace.

```
Object reference not set to an instance of an object =>   at System.Net.AutoWebProxyScriptEngine.InitializeRegistryGlobalProxy () [0x0005b] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.AutoWebProxyScriptEngine.GetWebProxyData () [0x00007] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.WebProxy.UnsafeUpdateFromRegistry () [0x0001a] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.WebProxy..ctor (System.Boolean enableAutoproxy) [0x0000d] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.WebProxy.CreateDefaultProxy () [0x00012] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.Configuration.DefaultProxySectionInternal.GetSystemWebProxy () [0x00000] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.Configuration.DefaultProxySectionInternal.GetDefaultProxy_UsingOldMonoCode () [0x00036] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.Configuration.DefaultProxySectionInternal.GetSection () [0x00015] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.WebRequest.get_InternalDefaultWebProxy () [0x00022] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at System.Net.HttpWebRequest..ctor (System.Uri uri) [0x0008d] in <ae22a4e8f83c41d69684ae7f557133d9>:0
  at (wrapper remoting-invoke-with-check) System.Net.HttpWebRequest..ctor(System.Uri)
  at System.Net.Http.HttpClientHandler.CreateWebRequest (System.Net.Http.HttpRequestMessage request) [0x00006] in <7ebf3529ba0e4558a5fa1bc982aa8605>:0
  at System.Net.Http.HttpClientHandler+<SendAsync>d__64.MoveNext () [0x0003e] in <7ebf3529ba0e4558a5fa1bc982aa8605>:0
  ```

A quick google took us here [https://github.com/mono/mono/issues/10030](https://github.com/mono/mono/issues/10030). A bug in Mono which can occur when a user has `ProxyEnable` in the registry, but no entry for `ProxyServer`. We ran the following in the command prompt to clarify the issue, but you can see it more easily by just going to Window's Proxy settings (hehe the command line is always the first place I end up).

```
reg query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyEnable
reg query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer
```

Anyhoo, they then disabled the manual proxy setting, clicked 'Save' at the bottom of the settings page, and booted up TaleSpire. Instantly it worked as expected!

This is great, but I now need to find a workaround we can put in the code. I'm hoping that [HttpClient](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclient?view=netframework-4.7) will allow me to disable the proxy. Then I should be able to look up the keys and force disable the proxy if it's the issue.

This is almost certainly not an issue in later versions of Unity, as they will probably be using a later version of Mono. This is a perfect candidate for the kind of issue that really makes you want to update the Unity version. However, that is always a bunch of work. We'll see how this workaround goes.

That's all I have for tonight. If all goes well I should be back on rendering code tomorrow.

Seeya
