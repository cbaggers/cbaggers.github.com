---
layout: post
title: TaleSpire Dev Log 294
description:
date: 2021-10-25 18:12:34
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

There has been a lot going on this past week, but just for fun, this morning, I took a few hours to look at performance.

The motivation came from a community member who posted some thoughts on performance issues they were seeing and shared the relatively [beefy board](https://talestavern.com/slab/dark-souls-megadungeon/) they were testing.

I cracked out the profiler and saw what I expected. The frame-time was dominated by shadow rendering. Due to *reasons*[0], the shadows are culled on the CPU, and because of the sheer number of tiles, this was taking a long time[1].

Poking around, I saw lots of things that are already on my todo list for future improvements. What I didn't expect was to spot something dumb.

`BatchRendererGroup`'s [AddBatch](https://docs.unity3d.com/ScriptReference/Rendering.BatchRendererGroup.AddBatch.html) method takes a [Bounds](https://docs.unity3d.com/ScriptReference/Bounds.html) which encapsulates all the instances. I had assumed that it would be used during culling to exclude batches that clearly didn't need culling. However, this wasn't the case.

Armed with this knowledge, I simply tweaked the culling job to check the bounds for the entire batch first, and only if it intersected the view frustum, to cull the individual instances. Naturally, this had a big effect.

When I first tested the board linked earlier, I was getting ~28fps. After this change, I was getting ~58fps. It dipped in some places in the board but never below 40fps, so this was still a nice win. [2]

![quite a few assets](/assets/images/dark60.png)

This will go out in a patch later this week.

While I was in the headspace, I also added some coarse culling to dynamic lights. It helped a little (and nudged the test board up to ~60fps), but doing more optimizing can wait for another day. [3]

Have a good one folks!



[0] Initially we tried to render all tiles via [BatchRendererGroup](https://docs.unity3d.com/ScriptReference/Rendering.BatchRendererGroup.html)s. This failed, however, due to the (undocumented) face that `BatchRendererGroup's were never meant to be used with Unity's built-in render pipeline, and per-instance data was simply not supported.

To tackle this, we use compute shaders to perform frustum culling and populate the draw lists for [DrawMeshInstancedIndirect](https://docs.unity3d.com/ScriptReference/Graphics.DrawMeshInstancedIndirect.html). However, when using `DrawMeshInstancedIndirect` Unity doesn't have enough information to do culling for you, and there are no hooks for doing this (In the built-in render pipeline, which we use).

So! We opted for a hybrid monstrosity. Shadows are handled via `BatchRendererGroup`, and we use our custom code to do the primary rendering.

`BatchRendererGroup` gives us nice hooks to perform culling, and we do these in Burst compiled jobs.

[1] This code no-doubt needs optimization too, but that's for another day.

[2] Naturally, your mileage will vary. The effect will be most visible in larger boards where a higher percentage of the board is off-screen at any given time. Also, I'm running an AMD threadripper on my dev machine, so it inflates numbers a bit. However, this change will improve performance on all machines regardless of CPU as it's simply doing less work :)

[3] The next big candidate for performance improvement is physics. I'm pretty confident that we can be smarter about what assets are involved in the simulation of each frame. Cutting down the number of assets included has the potential to help quite a bit.
