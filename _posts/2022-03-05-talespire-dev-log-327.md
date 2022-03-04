---
layout: post
title: TaleSpire Dev Log 327
description:
date: 2022-03-04 23:15:41
category:
tags: ['Bouncyrock', 'TaleSpire']
---

'Allo folks.

Yesterday seemed to want to stop me from working on Mac support, and it won. Today I turned the tables.

![running on m1](/assets/images/macAgain.png)

Before we get into that, let's skim through some tasks from earlier in the day.

The first order of business was handling the renewable of some server certificates. This led neatly into one of the problems from yesterday, certificate authentication callbacks failing on the latest Unity version.

It turned out that, while the [HttpClientHandler.ServerCertificateCustomValidationCallback](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback?view=netframework-4.7.2) callback is now broken, the global [ServicePointManager.ServerCertificateValidationCallback](https://docs.microsoft.com/en-us/dotnet/api/system.net.servicepointmanager.servercertificatevalidationcallback?view=net-6.0) one still functions.

I hate this global event as it is fragile to modification by other code[0] and is called for all requests when we only need it for specific ones. We only need to check requests to the backend against our pinned certificate, so I have to filter those cases in the callback. However, at least we can continue work.

I then updated our build scripts, so we could perform mac builds. As I previously had issues building directly to the network share, I now build to a temp directory and copy the files after completion.

Next was updating platform-specific code:

- Reliably obtaining the monitor size on Windows required some calls to the win32 API. For Mac, I'm starting by assuming the Unity commands will suffice and will write a native alternative if needed
- I updated the dylib for [miniz](https://github.com/richgel999/miniz) to one that supports M1
- Updated a couple of shaders `#ifdef`s to include them for Metal
- Disabling the pixel-picker on mac
- A whole slew of code house-keeping stuff

That was enough to get TaleSpire to start, login, and load a board. This felt huge as the last time I tried this, I couldn't make a standalone build, steam's API didn't support M1 mac, and I had run a hacker server locally just to get something running.

With all of this, I am finally ready to start coding the thing I tried to start midday yesterday :D

Well... almost ready. I just need to download XCode, and I sure do hate me some XCode. That's just the price of Mac support, I guess.

Anyhoo, that's enough for today.

Hope you have a great weekend.
Ciao.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] because, as the docs say, "Despite being a multicast delegate, only the value returned from the last-executed event handler is considered authoritative"
