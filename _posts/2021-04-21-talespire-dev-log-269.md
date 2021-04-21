---
layout: post
title: TaleSpire Dev Log 269
description:
date: 2021-04-21 13:25:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

> This dev-log is an update from just one of the developers. It does not aim to include all the other things that are going on.

Hi everyone!

This is my first one of these post-release, and it's been a weird first week, that's for sure. In some ways, the game has been much more stable than expected. The servers have held up well, and, from watching twitch, it seems a good bunch of people have been able to play, which is great. This meant I could take the weekend off and start the process of catching up on sleep. This was very welcome after the nervous lack of sleep caused by the approval delay.

Now I'm a bit more compos mentis I'm tackling bugs, focussing primarily on any that cause crashes or stop people joining boards.

One bug in line-of-sight is a bit of a blighter and which I'm now waiting on more information on. We have players for whom [supportedRandomWriteTargetCount](https://docs.unity3d.com/ScriptReference/SystemInfo-supportedRandomWriteTargetCount.html?_ga=2.214863124.2064915093.1619004981-1439509852.1590509612 is one, this means that when calling [SetRandomWriteTarget](https://docs.unity3d.com/ScriptReference/Graphics.SetRandomWriteTarget.html?_ga=2.150390197.2064915093.1619004981-1439509852.1590509612) you'd think you want to call with index zero. However, as the docs explain:

> The UAV indexing varies a bit between different platforms. On DX11 the first valid UAV index is the number of active render targets. So the common case of single render target the UAV indexing will start from 1. Platforms using automatically translated HLSL shaders will match this behaviour. However, with hand-written GLSL shaders the indexes will match the bindings. On PS4 the indexing starts always from 1 to match the most common case.

However, `SetRandomWriteTarget` checks if the index is greater-than or equal to `supportedRandomWriteTargetCount`, and if so throws an out of range exception. 

I'm fairly sure this means that the one random-write-target that `supportedRandomWriteTargetCount` is referring to must be the render-target. If so, it means that I'll need a new approach for line-of-sight on these GPUs. What a pain :D [0]

That aside, the next patch will have (amongst other things) a few small fixes to issues stopping players from joining boards.

Have a good one folks

[0] In future, we will be focusing on supporting lower-end machines. It is its own art, so we are trying to get the game proper functioning well first.
