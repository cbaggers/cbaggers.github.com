---
layout: post
title: TaleSpire Dev Log 190
description:
date: 2020-06-14 17:08:38
category:
tags: ['Bouncyrock', 'TaleSpire']
---

This week has been dedicated to research and planning, and it's been fantastic.

We have a lot of things that need to be done by Early Access. Between our plans, user requests, and polish, we definitely have our work cut out for us.

Luckily some features are also polish, probably none more so than performance. We've known for a long time that we would need to take some big steps to tackle performance issues. TaleSpire is a user-generated-content game, and these have some challenges which are not present in many games. These typically revolve around the fact that the scene can change dramatically at any time and so you cannot use techniques that require precalculating lots of things at build time.

There are tools which we've mentioned we wanted to look into, the big two being Unity's new ECS and Gpu occlusion culling.

### ECS

We started by performing some quick and dirty tests with entities by adding an entity conversion component to the tiles being spawned. We happily noted that in our first scene, the time spent on rendering noticeably dropped. There were some cases, however, where it didn't seem so cut and dry, and so we did some digging. It certainly feels like there has been a significant performance regression in the hybrid-renderer since the 'Mega City' demo unity was showing off last year. Based on the code, this seems centered around material properties. This was a bit disheartening but did teach me about the [BatchRenderGroup](https://docs.unity3d.com/ScriptReference/Rendering.BatchRendererGroup.html) class, which is likely going to become very important to us this year.

> Now, before folks get up in arms against Unity, this system is in preview, it's not shipping quality and the version in unity 2020 apparently already has significant performance improvements. This is just how software development is. We are just inside the sausage factory here.

Now it's very true that, given the same engineers, general systems will often be slower than one made for a specific task. And from my reading, it seems that, in TaleSpire's case, we can have opportunities to cache more heavily and get speedups based on assumptions we can make about our own data usage. We will dig into this over the coming months.

Another similar system is Unity's new stateless physics system. From our tests, we see very promising speedups compared to the current system. Once again, I believe there are places where we can feed the system in a way that can limit the impact of some tasks they have to do when starting the physics step for that frame. 

Now, changing the physics system means changing everything that uses the old system. That means board building tools, dice, and creatures, just to name three. And, let's face it, those three are a lot of what TaleSpire is :D We are going to have to work very aggressively on this to get TS back into a playable state as quickly as possible, but it's still going to take time.

If we can achieve both of these changes, we can eliminate the time it takes to spawn tiles. This means that we can remove the progressive-tile-load system, which is one of the most complicated parts of TaleSpire. Its whole job was to mitigate the fact that Unity's previous object-oriented approach resulted in us not being able to spawn many objects per frame. This meant that what you saw, and the actual state of the board data are necessarily out of sync. We managed that, but, as mentioned, making that robust took a lot of work. I'd love to remove it.

So that's all on the entities side. I've left out a lot of details and complexity, but I'm very optimistic that we can get see some big wins here.

### Gpu Occlusion Culling

Occlusion culling is the act of not drawing things hidden from view (occluded) by other objects in the scene. It's a very powerful concept but a reasonably complicated one, which often requires a lot of number crunching. Many games do this 'offline,' the level is analyzed by a program that precalculates what is visible from where. This would be done during builds of the levels and then would be used at runtime. As mentioned above, offline approaches are not ones TaleSpire can use, so what can we do? More recently, as GPUs have improved, people have worked out ways to achieve realtime versions of this on the GPU. It is very common in TaleSpire that people decorate the insides of builds, and none of those tiles usually need to be drawn when the camera is outside the building. In short, we might get substantial wins from occlusion culling.

I'd started implementing a GPU occlusion culler outside Unity before, but I'd lacked the Unity knowledge to know how to do it there without writing my own render pipeline. After seeing BatchRenderGroup, a bunch of stuff clicked, and now I'm pretty sure I know how to do this. As with my previous experiments, I'm basing my work on [this blog post](https://interplayoflight.wordpress.com/2017/11/15/experiments-in-gpu-based-occlusion-culling/), and a yesterday I wrote a MonoBehaviour that computes the hierarchical depth (Hi-Z) mip chain for a camera.

I've got a few things on my todo list for this week, but as soon as possible, I want to write a working test version of this so I can explore how far we can take this.

One side note from this is that we will be moving to use low poly meshes for all occluders. This means line-of-sight, fog-of-war, and shadows should all get a speed boost.

### Games Systems

As well as the research above, we spent a good deal of time exploring ideas for the following game systems.

### Terrain/Water

We have an initial idea for a system that we hope will balance a decent game experience with compact data representation. No details on it today, but Ree is currently prototyping to see if the idea will feel good enough for a v1 in the Early Access.

#### Props System

The prop system has been near to releasing at least 3 times over the last 2 years. We sat down and started by discussing the implementation details (as it's going to have to be made to work with all the changes mentioned above). However, as we talked, we grew increasingly concerned that the attachment-point based approach might not be good enough for handling the kinds of cases we had seen watching people build. We went back and forth on this for a bit and realized that we needed to prototype the tooling and see what it would end up feeling like. The last 20% of making something often takes 80% of the time, so it's far better that we convince ourselves now than to build the wrong thing.

#### Locking the Unity version
Our modding system relies on Unity's prefab format. Once people can make mods, we need to make sure that we don't break mods by upgrading to a Unity version, which uses an updated prefab format. To us, this means we need to pick a Unity version and stick with it for a long time. Naturally, that means we can't take advantage of the newer Unity feature or fixes, so we have to be sure that the version we have is something we can stick with. We'll keep you posted on this too.

### Wrapping Up

I'd like to talk on stream a bit about this stuff and a selection of the feature-requests. We won't have timelines for the requests, but we can at least address what we see going into TS and what won't. At that time, I'll release the list of feature requests. I'm not going to do it before as the notes need cleaning-up and that work takes a lot of time.

Alright, that's it for now,

Seeya tomorrow!
