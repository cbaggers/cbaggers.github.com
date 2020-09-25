---
layout: post
title: TaleSpire Dev Log 228
description:
date: 2020-09-25 17:00:21
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Since our move away from GameObjects, we have had to make our own batching, culling, and animation systems. One that I had left for a while was lighting, so it was time to tackle that.

First off, the deeply annoying news. Unlike with the BatchRendererGroup, which gave us a way to avoid GameObjects and populate the data from multiple threads, there is no similar thing for lights. In fact, when I looked into Unity's ECS, they were just spawning GameObjects for the lights and updating their transform each frame to be in line with the entities that supposedly contained them.

This is a pain, but it's the only option, so we needed something similar.

As always, the fact that TaleSpire is a user-generated-content game means we have a bunch of complexity. We don't know in advance how many lights are about to be spawned. Worse, because we are back using GameObjects, we know that spawning too many per frame will cause serious fps issues. This means we need a progressive spawning approach and pools for reusing light that that been destroyed.

Also, spaghet scripts are allowed to update light color, intensity, position, and rotation on a per-frame basis. Now position and rotation updates can be jobified if we put the light's Transform in a TransformAccessArray. However, color and intensity can only be set from the main thread... joy >:[

On each change to a zone, we rebuild all the batches, including laying out the lights again. We have jobs to:
- find out what tiles and pros are using lights
- collate the numbers of each kind of light
- write the details for each instance of each kind of light (in parallel) into various collections.

We push all of the current lights back into the pools for reuse and, over the course of multiple frames, spawn new lights or pull them from the pools.

Dynamic lights are updated by Spaghet scripts. If there is a change to the intensity or color, we push the index of the modified light into a NativeQueue. That queue is consumed on the main thread where we can apply the changes.

Getting all of this to play nice has taken me the last 3-4 days. There is still plenty of room for improving performance, but I need to profile using real-world scenes before making any more changes. Luckily we have fantastic community sites full of slabs I can use :D

Alright, the next stop is looking at little things that broke on the move to the new physics engine.

Seeya in the next log (which will hopefully take much less than a week this time :P)
