---
layout: post
title: TaleSpire Dev Log 246
description:
date: 2020-12-10 17:01:06
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey again folks.

Progress has been good on the new mesher for the fog of war. The new mesh is much more regular, which should help if we use vertex animation on the mesh.

In the clip below, you will spot two significant issues (Ignoring the shader as we haven't started on that yet):

- The seams in the fog at the edges of zones (16x16x16 world-space chunks)
- The lighting seems weird

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=TXqvB1vlpw8" frameborder="0" allowfullscreen></iframe>

The lighting oddness is just that, for now, all the normals are set to straight up. I'm going to make them per-face after this. I could compute the normal per-vertex, but as that slightly more work, I'm holding off until we have some ideas about the visuals.

The seams are an artifact of marching cubes and keeping zones separate from each other. As much as possible, we want to keep zones independent from other zones, and in this case, it means we don't know if the neighboring zones have fog or not. This, in turn, means we assume there is none so that marching-cubes generates a face. However, marching-cubes can't do sharp corners, so you get this chamfer.

I'm not sure if we can fix this by modifying the geometry generated on the edges. We'll have to see.

This morning I was fighting with Unity, trying to get it not to look for things to cull in cases when we are handling the culling. Without moving to their new "scriptable rendering pipelines" (SRP), there doesn't seem to be a way to do it.

I will also look at the shadow culling jobs as I think the overhead from dispatching them might be larger than the time the job is taking [0]. In that case, I could Burst compile the culling methods and call them from the main thread instead. It's a trade-off, but it might work.

That's all for now. Hopefully, I'll be back tomorrow with some new data :)

Peace.

[0] The profiler seems to suggest this is happening, but I'm not sure how much of the overhead is avoidable and how much is just part of the BatchRendererGroup's own code.
