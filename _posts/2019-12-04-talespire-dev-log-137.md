---
layout: post
title: TaleSpire Dev Log 137
description:
date: 2019-12-04 16:03:31
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey! I best recap the last two days before even more get away from me.

### Monday

Monday was spent debugging some of my uses of unsafe code. Unity's Job system has some great tools to detect race conditions, but they also let you disable it if you have a use case that demands it. This is a wonderful attitude, and I'm very grateful for that. However, boy can I corrupt some memory with this stuff.

The first thing was debugging a new NativeCollection I had made. This is a Stack with a fixed max size that can have multiple concurrent writers or readers (but not both at the same time). I use this to hand out and return large numbers of Ids to a limited resource, and to do so from concurrent jobs.

The id, in one case, is an index into a big array that stores state that is later pushed to the GPU for use in the shaders. Each visible tile has one entry in this array and gives that spot back when their presentation is destroyed. This can happen with large numbers of tiles across multiple zones simultaneously.

Because each tile gets a unique index into that array, we know that they will be the only thing accessing it, so we don't want Unity to protect us from race conditions that would occur otherwise[0]. This means we add the `[NativeDisableContainerSafetyRestriction]` attribute.

That, of course, allows you to really make a mess, and sure enough, I did :p.

In this case, it wasn't due to that attribute, however. I was using [MemCpyReplicate](https://docs.unity3d.com/ScriptReference/Unity.Collections.LowLevel.Unsafe.UnsafeUtility.MemCpyReplicate.html) to set the default shader state across a portion of the array and I *may* have incremented the pointer to the strut rather than the pointer into the array. So I happily tried to copy invalid portions of memory into the array.

I was actually super lucky that this caused Unity itself to crash very consistently. These kinds of bugs are horrifying if they stay under the radar.

With that done, all tests passed again, and I got back to work.

### Tuesday -> Wednesday morning

Tuesday started with traveling to visit @Ree. It's always great when we get to do this as certain kinds of tasks are so much easier.

In a related subject.. yesterday I was working on the line of sight shader :)

This starts with rendering the scene into a cubemap from the head of the creature you have just placed. Each thing rendered is colored based on an id (zero for all occluders and >0 for creatures or potentially other points of interest). We then run a compute shader to sample the cubemap and aggregate which ids are in there.

To start with, we want to store 32bit ids, which meant using a floating-point texture format. This seemed to work, but every time I read the value from the compute shader, the value was always clamped between 0 and 1. 

It's my first time doing this in Unity, so I spent ages trying to find out what I'd done wrong until I gave up and ran [renderdoc](https://renderdoc.org/) on it. I should remember to do this first as it turned out Unity was rendering the faces in non-float textures and then copying the data over :|

[RenderToCubemap](https://docs.unity3d.com/ScriptReference/Camera.RenderToCubemap.html) doesn't seem to warn about this, but @Ree got me on the right path by saying we should try a RenderTexture instead. RenderToCubemap has an overload for this, and there is even this interesting line in the docstring for the overload that takes the cubemap directly 

> If you want a realtime-updated cubemap, use RenderToCubemap variant that uses a RenderTexture with a cubemap dimension, see below.

I'm guessing that this is a hint to the intermediate copies (and maybe a temporary FBO too?).

After moving over to that, I started getting values higher than one out of the texture YAY! The values are wrong, but frankly, I don't care about it today. The pieces of the puzzle have all been started, and I can work on those from home. It's best to use this time I have over here to touch on as many other things as possible.

## The rest of today

There have been more chats working out details of performance around tile hide and cutaway and much musing on how to merge out branches in a way that doesn't slow us down.

That's my next task, to update the main dev branch to use Unity's [assemblies](https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html) and begin moving in the new data model code. 

We have to be very careful though as it's soon Christmas and we really mustn't block either us from being able to work as we won't be available to help each other during that time.

That's all for now.
Seeya!

[0] We aren't, in this case, too concerned about [false sharing](https://en.wikipedia.org/wiki/False_sharing) or other forms of contention that may hurt us. This will get more of a review later, however.
