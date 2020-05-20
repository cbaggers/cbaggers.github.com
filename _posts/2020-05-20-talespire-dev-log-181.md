---
layout: post
title: TaleSpire Dev Log 181
description:
date: 2020-05-20 02:17:14
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks, another day of slow but steady progress on the fog of war. What I did today was hook up the code that queues up mesher jobs when the underlying fog data changes. I can now select a region, press a key, and the fog volume data for all the zones in the region is updated. The presentation for those zones then enqueues a job that reads the volume data and generates a minecraft-style mesh that will (in time) be used for fog. As, in this test, the selection tools work with cuboids, the fog only follows that shape, but we can see that the resulting mesh is reasonable (lower poly compared to the worst case), and I can see that the jobs are being run in parallel. So far, so good.

![mesh](assets/images/fogMesh0.png)

Now, this is done I can write the code that takes the cubemaps captured from the line-of-sight system that handles hiding creatures, and passes them to a new compute-shader which will make an update mask of what areas are visible. I'll be doing this on the GPU as:

- The data I need is already on the GPU which means no having to shift it around
- The approach I'll be trying is trivial to parallelize without divergence and so should be very suited to running there.

From the CPU side, we will then wait on a GPU-fence (which tells us when the compute-shader has finished), read the result from the compute-buffer, and apply it to the fog data in the zone. This change will then trigger the code we wrote today and we should see the new mesh which will hopefully have removed all parts that were in view. It will absolutely be rough, to begin with, but with the full round trip written, we can move onto playing with it and making it better.

I'll go into more detail on the approach once I've got it working as it should be pretty simple.

Alright, seeya around.
