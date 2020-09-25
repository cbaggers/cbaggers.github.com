---
layout: post
title: TaleSpire Dev Log 227
description:
date: 2020-09-25 16:05:02
category:
tags: ['Bouncyrock', 'TaleSpire']
---

This one is interesting to write as it's fairly downbeat. However, it is part and parcel of making stuff, so let's do it.

As mentioned in previous posts, we have known for a while that TaleSpire would not scale to bigger boards using their GameObject abstraction. There was a lot of song and dance from Unity about their upcoming ECS, and while it looked pretty promising, we miscalculated the timeframe it would take Unity to bring it to life. That meant that this year, as we tested again, we saw we needed a custom solution. That's what I've been working on.

Now I have tried hard to always be clear about when the issues we have run into have been Unity's fault and when they have been ours. In general, the responsibilities have been ours as we are using packages marked experimental and not in general recommended for shipping. However, today's principal issue is, I believe, not on us.

To move away from using GameObjects we have been building on the BatchRendererGroup. This object lets you specify what needs to be drawn and then provide the matrices to lay these instances out in world-space. The matrix array you fill is a NativeArray and, as such, is fully compatible with the job system[0]. This meant that we have been able to parallelize the population of these arrays, which has resulted in the massive improvements in the performance spawning tiles we have seen in previous dev-logs.

When you are using instanced rendering, you need to somehow get the per-instance data to your shaders. Traditionally in Unity, this is done using MaterialPropertyBlocks. You can associate a MaterialPropertyBlock with an object and, when it renders, that data is made available to your shader. When using BatchRendererGroup, this requirement seems to be filled by using GetBatchVectorArray (and co).

Now here is my portion of the fuckup. When I couldn't get this to work in my early testing, I put it down to me not being too hot with Unity's shaders, and that I would work it out later. Unlike the new stuff in Unity, the BatchRendererGroup is not marked as experimental as so I had the same confidence in the system that I do for other released parts of Unity. I put a few questions up on the forums and got back to work[1]. I rigged up a simple system where I looked up the data from ComputeBuffers using the instance-id as the index; this works fine without culling enabled[2].

The problem is that GetBatchVectorArray does not (as far as I can tell) work with the classic rendering system in Unity. This is not stated in the documentation, but in the docs for an experimental package called the 'Hybrid Renderer,' it mentions some things that can be construed to mean it only works with the new scriptable rendering pipelines.

UPDATE: While writing this log, I've seen that my bug report against this method has been accepted. I will keep you posted on how this goes.

This sucks. Worse, we know the old version won't work; we are months into the rewrite with no way to go back. Frankly, it was rather distressing.

Now those of you who know a little about Unity might be wondering if we could just make something custom using DrawMeshInstanced or DrawMeshInstancedIndirect. The problem is that things drawn with those methods are not culled, and this is a problem as realtime lights/shadows require rendering the scene from multiple vantage points. Without culling, you end up rendering everything for each camera regardless of the direction it's facing. This massively hurts performance. The BatchRendererGroup was excellent in this regard as it allowed us to implement the culling jobs ourselves.

One thing that the BatchRendererGroup does have is a flag that lets you specify that you only want to use it to render shadows. Another little detail is that TaleSpire's shadows don't need to respond to the tile drop-in/out animation we implement in the vertex shader. This gives us the possibility to keep using the BatchRendererGroup for shadows and keep the culling advantages.

However, that still leaves actually drawing the scene. The only thing I could think of was that I'd need to implement this myself. I'd use all the batching code we have written to layout data on the GPU, I'd write a GPU-frustum culling implementation that matched the one we use on the CPU side, and use DrawMeshInstancedIndirect to dispatch the draw calls.

Now I've never written this kind of thing before, but it felt like the only feasible option. Over a few days, I got this written and was finally able to cull our scenes again without horrible flickering.

Now the scary thing is that this was not part of the plan. It's cost us a lot of time and how it scales is currently unknown. What I do know is now we have a whole pile of extra complexity to manage and improve. Not great.

However, there are some up-sides:

Currently, we have to use the same meshes for rendering, occlusion checks, and shadow rendering. I wanted to use different meshes for shadows and occlusion checks as this allows us to use lower-poly meshes (helping performance) and close tiny cracks the fog of war shouldn't be able to see through. I was implementing that as part of the GPU-occlusion system. When I delayed that feature to after the Early Access release, we delayed the 'separate shadow mesh' feature too. With our new system, we can trivially use different meshes, so these improvements are now on the cards for Early Access.

As mentioned recently, most of Unity's API has a max of 500 instances per batch when using non-uniform scaling. DrawMeshInstancedIndirect does not as all the data is already on the GPU. This means we have the potential to have much larger batch sizes. Currently, we batch on a per-zone basis, so we are a little limited, but it will not be hard to take zones that have not changed in a while and combine their batches. It's something I'll look into when I get back to improving performance.

Thirdly, we now have a bunch of tile/prop data on the GPU that we can easily write compute-shaders to operate on. This gives us more options for future developments.

So all in all, this has been pretty horrible. I'm very grateful that the community is so supportive and that the Eldritch Foundry update was so well received. That was a serious boost in a tough couple of weeks.

However, we are still very far from done. By my estimates, I was at least a month behind my personal goals before this escapade, so I'm not sure where we are now. Luckily we've been vague with deadlines, but we will need to sit down and look at the plan again.

What a year!

This dev log is long, so I'm gonna leave talking about lighting to the next post.

Seeya there

[0] This is important as, traditionally, most of Unity' apis are only valid to call from the main thread.

[1] There is such a lot of work to do for Early Access that often I can't afford to stress on issues that can be worked out another day. The important thing is to be making progress somewhere as often another day I'll have had time for the solution to come to me.

[2] Culling means some instances are not being drawn and so the instance-id for a given tile can be different from frame to frame.
