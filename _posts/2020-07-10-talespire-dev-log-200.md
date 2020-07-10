---
layout: post
title: TaleSpire Dev Log 200
description:
date: 2020-07-10 21:57:52
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

On Thursday, I continued work on the batching code. I got a first approximation written[0], but upon trying to hook it into the existing setup, I noticed that disabling the current system is not trivial. In fact, I had a choice between working a bunch for a test that wouldn't teach me much[1] or just plowing into the real work of replacing the current system. I chose the latter.

This means that the goal I set for myself of "something rendering by the weekend" was not feasible, and I shouldn't push myself unnecessarily. The task of replacing the system touches a lot of code and requires a different mental approach.

So instead of fretting Friday, I decided I'd take it off and just work Saturday instead. One of the blessings of working for yourself :)

Because of that, I have no news for today. I also expect that the updates from me for the next week will be variations of "Replacing part X of the old tile system". However I will try to keep the posts coming in case anything interesting crops up.

Until then,
Bye!


> [0] Approximation as it just positions all the parts of the tile at the tile's origin. So it'll be a mess but will show if things are working. I did this as the math requires a focus to make sure I don't screw it up and should be taken slowly.

> [1] I have already made experiments to learn how the BatchRendererGroup behaves, and the process of writing the batching code had given me a good deal of the context I had hoped to get from the exercise.
