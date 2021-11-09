---
layout: post
title: TaleSpire Dev Log 297
description:
date: 2021-11-08 23:23:50
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today my goal was to take the HeroForge asset processing code and turn it into the state machine used in TaleSpire. 

This has gone reasonably well. The code is split up into a few steps, and the texture processing jobs seem to be running across many frames as hoped. This should have meant that our code was barely blocking the main thread. However, I got to learn something new instead!

It turns out that [CreateBlobAssetReference](https://docs.unity3d.com/Packages/com.unity.entities@0.7/api/Unity.Entities.BlobBuilder.html#Unity_Entities_BlobBuilder_CreateBlobAssetReference__1_Allocator_) is slow, really slow, like 100ms to allocate slow. This really surprised me until I realized that, while we load BlobAssetReferences in TaleSpire (and they load fast), we only create them in TaleWeaver, where subsecond delays don't feel bad.

It's only a minor issue, though, as we can just knock together our own little format. This will take a few hours tomorrow, and then I can look at the performance again. It remains to be seen how these long-running jobs will interact with the per-frame work that TaleSpire already has to do, but we'll be able to check that once we get this all hooked up.

The really slow part is not our code, though, but [GLTFast](https://github.com/atteneder/glTFast). On my machine, it's taking 54ms per frame over 3 frames for it to load the data, and my machine is no slouch. I'll definitely be looking into what we can quickly do to improve this[0], but not yet. I want to get all this code into TaleSpire and start wiring things up so I can see the gaps that still need filling.

Although Ree and I usually blog about our own stuff, I'm going to shout out his work today as I find it exciting. Ree's currently refactoring the camera controller code, which is the kind of nuts-and-bolts improvement that has been stuck on our todo lists for ages. Not only does it make the codebase easier to work with, but it also takes us measurably closer to being able to give you all access to the cutscene tools that we use to make our trailers. The idea naturally being that you would be able to play these to your players in a session. 

Alrighty, that's the lot from me today.
Seeya tomorrow.


*[0] Hopefully, I've just not read the docs properly, and there'll be some "skip horribly expensive operation" option. But if not, I'll have to look at having it create views into the data rather than Unity objects and handle the raw data myself. Then at least we can kick it off to a separate thread and wait for it to finish.*
