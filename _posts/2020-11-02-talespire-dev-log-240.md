---
layout: post
title: TaleSpire Dev Log 240
description:
date: 2020-11-02 16:37:33
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

As you may have seen, the dice URL support just shipped today.

This post will be in two parts. The first is about how dice URLs are formatted, and the second part will be the usual grumbling about bugs :P

### Url Anatomy

All TaleSpire URLs start with `talespire://`. From there on, we have some number of path segments.

We call the first path segment the "behavior identifier". The behavior identifier tells TaleSpire how to interpret the rest of the segments.

For example, in `talespire://dice/d12`, `dice` is the behavior identifier and tells TaleSpire to interpret the rest of the path as a dice roll.

> Currently, we only support basic dice rolls with the `dice` behavior. In the future, we will support more complex dice rolls as well as other different behaviors.

Next, let's take a more complex URL and break it down. We'll use this as our example: `talespire://dice/1d12/4d6-1d4/4d10+2/3d20+1-d6+2`

First, we'll ignore the scheme and behavior identifier, which leaves us with these path segments: `1d12/4d6-1d4/4d10+2/3d20+1-d6+2`

Each path segment specifies a "dice group". The roll results of each dice group get totaled separately. You can see an example result from the above roll here:

![multiple results](/assets/images/diceGroupsResults.png)

A dice group is parsed, right-to-left, case insensitively, with the following regex: `(\+|\-|)\d*D\d+(\+\d+|\-\d+|)`

The C# code to do this is:

```
var diceGroup = Regex.Matches(pathSegment, @"(\+|\-|)(\d*)D(\d+)(\+\d+|\-\d+|)", RegexOptions.RightToLeft | RegexOptions.IgnoreCase);
```

The extra brackets in the C# regex above were added so that the `Groups` field contains the most useful data already separated. Naturally, you will need to reverse the results to get them in the correct order.

The regex is the most accurate specification, but here is a simpler (though maybe less precise) version with extra details.

> [optional operator][optional count]D[sides][optional modifier]

where:

- `operator`: currently, the only operators supported are `+` and `-`.
- `count`: is the number of dice of that kind. Zero is not valid, but if it is not specified at all, then the count is one.
- `D`: is only uppercase in the regex to clearly distinguish it from the `\d`s. The regex is always case insensitive.
- `sides`: is the number of sides of the die. Currently, we only support the standard TaleSpire dice. We will need to expand to support dice modding.
- `modifier`: here, you can specify an integer to add or subtract. It is always of the form `+N` or `-N` where `N` is a positive integer

Three points of interest:

*Negation*

As the operator is part of the group, a valid dice URL is `talespire://dice/-d6`. This results in a negated dice roll.

Note that `talespire://dice/d4-d6` is supported but `talespire://dice/d4--d6` is not.


*Righ-to-left*

The right-to-left matching is critical to get the correct result:

Parsed left-to-right `talespire://dice/d6-2d12-1` matches as `d6-2` and `d12-1`, rather than what we want which is `d6` `-2d12-1`

You can experiment with the right-to-left option by browsing here https://www.regexplanet.com/share/index.html?share=yyyyd9vu5ar and clicking the `.Net` button.

> Note: remember to example case insensitive on regexplanet before clicking the 'Test' button


*Garbage is ignored*

Due to the lack of strictness in the specification, it is valid, although ugly, to have ignored text in the URL. For example: `talespire://dice/horsed6horse-2d12-1horse` is parsed to `d6` and `-2d12-1`.

Arguably this should be made stricter.



### Normal dev log

With that out of the way, time for me to grumble. Getting this feature out has been a huge pain in the ass. To explain why I need first to describe how this is set up, don't worry, it's super simple.

- TaleSpire adds a registry key on install that tells windows what exe to run when a `talespire://` URL is launched.
- We don't want lots of copies of TaleSpire starting, so instead, we have `TaleSpireUrlRelay.exe`, which either
  - launches TaleSpire and passes the URL as a command-line argument
  - use some form of [IPC](https://en.wikipedia.org/wiki/Inter-process_communication) to send the URL to the running instance of TaleSpire

One of the testers had an issue where the URLs would not arrive unless the TaleSpireUrlRelay was set to run as administrator. This was clearly a permissions issue, and so I started exploring the security options of the IPC I was using. This is where things get screwy.

But first, let's wind back and look into how we got here.

> Note: this account is intended as entertainment. I likely missed very obvious information along the way and misinterpreted errors that would have told me the real issue. I can't accurately say which systems actually had bugs, but I can tell the little tale of how my week has *felt*. Enjoy!

So back in the day you might use [SendMessage](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendmessage) (or PostMessage maybe) with [WM_COPYDATA](https://docs.microsoft.com/en-us/windows/win32/dataxchg/using-data-copy). However, since Vista, this `WM_COPYDATA` is blocked for security reasons. However, [there seem to be workarounds](https://www.codeproject.com/Tips/1017834/How-to-Send-Data-from-One-Process-to-Another-in-Cs) but this looks nasty, and it would be great to avoid this if possible.

Also, we want to port to Linux and Mac in the future, so something in the .Net framework would be much better. You could use a socket, but that's very low level for what should be a straightforward thing.

How about websockets? We already have a client in TaleSpire, and we have stated we would like to expose a websocket API in the future. However, that has almost the opposite problem. It's a huge piece of extra 'machinery' running in the game, and we don't know how performant it would be. It would be much nicer to have something lightweight and then launch the websocket server only when needed. So let's try and avoid the websocket approach for now.

Next, you land on [Named Pipes](https://docs.microsoft.com/en-us/dotnet/standard/io/how-to-use-named-pipes-for-network-interprocess-communication). They look ideal, and in fact, you get them working fine for you. However, this is when that tester I mentioned reported the permissions issue. Dang.

You have a read of the documentation and see the [PipeSecurity](https://docs.microsoft.com/en-us/dotnet/api/system.io.pipes.pipesecurity?view=dotnet-plat-ext-3.1) argument to the server constructor. "Yay," you say, "this should be easy." You take the example code from the docs, but it uses `TokenImpersonationLevel.Impersonation`, which seems to error in your setup. Ugh, maybe it's best to read some more rather than copying more code.

A quick google, and you start hitting some [scary](https://stackoverflow.com/questions/50711518/attempted-to-perform-an-unauthorized-operation-when-calling-namedpipeserverstr/54896975#54896975) [posts]() which seem to suggest that it just won't work... and you miss the fact that they seem to be about [.NET standard](https://docs.microsoft.com/en-us/dotnet/standard/net-standard) so maybe it's not relevant.

However, you have a lot of experience in cases where Mono doesn't implement something normally available in .Net, so it all seems feasible.

A bunch more testing later, and the only pattern you have found is that the Named Pipes just don't work if you try and specify the PipeSecurity. It doesn't help that every tutorial you find has a different way of setting things up. Is the SID meant to be `WellKnownSidType.AuthenticatedUserSid`? or `WellKnownSidType.BuiltinUsersSid`? or `WellKnownSidType.WorldSid`?, or `"Everyone"`, etc, etc.

Ok, so now we are worried that Named Pipes won't work, what other options do we have? Hmm, [IpcChannel](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.remoting.channels.ipc.ipcchannel?view=netframework-4.7.2) looks promising. But, surprise surprise, when you copy over the code, it hangs. Now you have two implementations you don't understand. Do you really want to keep down this road?

Maybe you do for a couple more hours, but "damn," you think, "I just need something simple." Perhaps you could dump the URL into a file and have a file-watcher in TaleSpire pick it up. Ugly as hell, but at least it could work.

You keep that in mind and look at the next option in the list, MSMQ. Oh wait, according to a blog, it's ["dead"](https://particular.net/blog/msmq-is-dead)... well, not actually dead, but you don't need more confusion in your life, so after a quick read, you try to find something else.

Maybe [.Net Remoting](https://en.wikipedia.org/wiki/.NET_Remoting)? It's old as hell, but at least that means it isn't changing all the time. You dive into some examples, and it's built around pretending that the same object exists in both programs simultaneously. This model is gross, but hey, if we can get an example working, then maybe it's... nah, we are running into issues connecting too.

There is this horrible balance you are trying to strike. You can try and implement a system using the tech you don't understand, knowing that everyone gets stuff wrong the first few times, OR pull in some code someone else wrote and then have to learn that. Either way, this feature is taking way too long, and you desperately want to get it working, so you don't want to put days into exploring each of these options.

Terrifyingly the `SendMessage` stuff is seeming like something you might have to reconsider. However, after checking out some example code and some [fairly](https://github.com/wgraham17/simple-ipc) [promising](https://github.com/maintainnothing/NLocalIpc) [libraries](https://github.com/cgillum/pipe-server) you say "screw it" and go back to Named Pipes as that is the closest you've gotten so far.

BUT WAIT. There is something called AnonymousPipes, specially made for only local connections, with a simple API and. NOPE.
It only works if the server process launched the client process, which is not the case for us. Ah well, back to Named Pipes

Now you are back where you started, poking at seemingly random things to find out what will happen. While flailing around, you decide to give the connecting client [FullControl](https://docs.microsoft.com/en-us/dotnet/api/system.io.pipes.pipeaccessrights?view=netframework-4.8) of the pipe. You didn't try this before because you never want to grant anything higher permissions that it needs but suddenly, IT CONNECTS! You franticly whittle down the permissions until it's the smallest set that allows the connection to work and then make a new build and give it to the tester who had the issues.

Same problem.

Just as you are about to pour a giant glass of the most potent drink you own, you casually ask them if there is any way they know that the game could be running as administrator.

"Oh..." they say.

It turns out that when they were helping test things for you half a year ago, they tried setting the exe to run as admin, and Steam, being a good little soul, maintained this flag through every update until now.

They remove the "Run as administrator" flag, and immediately the dice-URLs start working.

Now. This is not a story about where the tester screwed up; these things happen. This is a story of the sea of nonsense you sail through trying to work stuff out when almost no information you find online is truly reliable, and you don't have time to learn it all from first principles.

In conclusion, this is why the URLs are three days late :D

That's enough stupidity for one night. Seeya folks!
