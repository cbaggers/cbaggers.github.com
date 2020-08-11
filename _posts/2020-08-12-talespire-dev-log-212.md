---
layout: post
title: TaleSpire Dev Log 212
description:
date: 2020-08-12 00:47:53
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today was pretty simple. I spent the day reading into the Unity.Physics package and trying to work out more of the nitty-gritty of how we will work with it. Tomorrow I'll start implementing things.

The first thing to do will be to work out the serialization for colliders on assets. I'll be slightly changing the data stored per object inside assets to avoid additional processing on the TaleSpire side. I'll then be looking at the colliders that live on tiles and are used by the building tool.

It'll then be over to Unity to look at loading this stuff and how to asynchronously create the mesh-colliders. I'll need to ensure that the other systems aren't notified that the asset is loaded until this processing is complete.

I'm sure I'll run into some other little complications on the way, but that's fine.

I expect this will take most of tomorrow, and so on Thursday, I'll start updating the batcher to handle the physics data. Colliders attached to animated objects will likely require some attention to get right.

With all that in place, it will hopefully be time to write a little helper API and to start updating all the existing code to use this new system.

We'll keep you posted on how this goes.

Seeya
