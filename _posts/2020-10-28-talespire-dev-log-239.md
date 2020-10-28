---
layout: post
title: TaleSpire Dev Log 239
description:
date: 2020-10-28 01:45:16
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya everyone,

I got back from my break on Sunday, fully rested, and ready to build again!

For the last two days, I've been getting back up to speed by looking at the custom URL scheme code again. [The last time I was working on this](https://bouncyrock.com/news/articles/talespire-dev-log-234), I forgot that the custom URLs always try to open a new copy of the application they are linked too. That's fine when TaleSpire is closed but no good if you already have it open.

To handle this, I wrote a little program that acts as a middleman for the custom URLs. It is called when one is used and then looks to see if TaleSpire is running. If it is, it connects to it using a [named pipe](https://docs.microsoft.com/en-us/dotnet/standard/io/how-to-use-named-pipes-for-network-interprocess-communication) and passes along the URL. If TaleSpire is not running, it launches it passing the URL as a command-line argument, and that argument is processed once the user has logged in.

In the future, we still think that we will expose an API for tools to hook into TaleSpire via websockets. However, that is a bigger task than I wanted to take on today, and I wasn't sure of the performance implications. By default, I think that the websocket server would be disabled, and we'd use one of our `talespire://` URLs to enable it and to fetch the connection details (like the port number).

As a test of this new code, I knocked together a simple (and rather unfinished) extension to TS that uses the custom URLs to spawn dice. It supports multiple totals within one roll and modifiers on every dice descriptor. For example, `talespire://dice/2D12+2+1D20` spawns 2D12 with a +2 modifier and 1D20 and will sum them. `talespire://dice/2D12+2/1D20` spawns the same dice, but the totals for 2D12 with the +2 modifier are calculated separately from that of the 1D20.

![custom url spawning dice](/assets/videos/customUrlDice.gif)

As you can see from the gif, there are still lots of things to fix. But it's coming together. Once it's working and we've play-tested (and we are sure we want to ship it), we'll publish the specification for the dice URLs.

That's all from me for today.
Seeya!
