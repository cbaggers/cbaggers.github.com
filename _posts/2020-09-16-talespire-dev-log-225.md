---
layout: post
title: TaleSpire Dev Log 225
description:
date: 2020-09-15 20:22:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks. For the last few days, I've been looking at batching again. Unity has an interesting limitation in that the max batch size is 1023. This seems to be an artifact of some decision long ago, however, we need to make sure we don't try and make batches bigger than this. Making our code respect this is a delicate operation as it's very multi-threaded code and the access to the data are often not protected by Unity's safety systems (for performance reasons). This means I've been taking it very slowly, it's going well, and I hope to have it finished soon.

As that is quite a short update, here is something from last week.

There is a node in Spaghet for choosing a mesh from a list. We use this for the stop-motion style effect of the fire. It looks like this in the graph.

[multi-mesh](multi-mesh-node.png)

While fixing bugs in the implementation, I had to fix things both on the TaleWeaver and TaleSpire sides, and the process got very tedious. At some point, I decided to re-prioritize work on the modding tools and hooked up Spaghet such that it works as a live previous in TaleWeaver. Here it is in action. Notice that every time we save, the changes are immediately applied.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ybnJR2_FxR8" frameborder="0" allowfullscreen></iframe>

This sped up the process a lot! I was able to find some mistakes in my animation packing code and fix those before getting back to batching.

Alright, that's the lot for this update.

Peace.
