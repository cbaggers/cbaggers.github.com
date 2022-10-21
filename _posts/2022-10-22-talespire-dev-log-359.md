---
layout: post
title: TaleSpire Dev Log 359
description:
date: 2022-10-22 00:08:46
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks.

After the push to get group movement into Beta, I've taken this week a little easier.

I've been quietly working away on the performance of the lasso picking, and I wanted to show some profiler screenshots, but I'm still chasing down some bugs, so I'll save that for another day. The issue goes like this:

- We need to render creatures for the lasso picking
- When we enqueue them using a [CommandBuffer](https://docs.unity3d.com/ScriptReference/Rendering.CommandBuffer.html), Unity doesn't "batch" them into an instanced draw call, so we end up with a lot of draw calls
- We can perform a [DrawMeshInstanced](https://docs.unity3d.com/ScriptReference/Rendering.CommandBuffer.DrawMeshInstanced.html) call ourselves, but then we have to handle the per-instance data ourselves as we are drawing a mesh, not a Unity [Renderer](https://docs.unity3d.com/ScriptReference/Renderer.html)

We could use a [MaterialPropertyBlock](https://docs.unity3d.com/ScriptReference/MaterialPropertyBlock.html), but doing this is extra work. How about we do something nastier. We already have to provide a Matrix4x4 transform for every instance, but only three rows of that matrix are being used. So let's sneak the extra data in the last row. This will absolutely break the world-to-local matrix Unity will generate, but we aren't using that, so hopefully, we can ignore it. With this, and some bodging in the shaders, we get the data we need to the GPU and MASSIVELY reduce the number of draw calls.

For example, this screen goes from over 400 picking related draw calls to about four.

![some guys](/assets/images/someCreatures.png)

The scene might seem contrived, but higher numbers of the same creature are expected when dealing with armies.

During the above work, I also spotted that I could accelerate the rough culling we do when picking creatures normally. Most won't notice this, but it definitely makes a difference on lower-end machines. This is part of the required work for the macOS release, so I'm happy to be doing it.

Alright, I'm very sleepy, so I'm heading off now.

Have a lovely weekend.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
