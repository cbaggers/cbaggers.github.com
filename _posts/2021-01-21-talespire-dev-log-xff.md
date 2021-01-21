---
layout: post
title: TaleSpire Dev Log #xFF
description:
date: 2021-01-19 14:20:47
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, 

Lots of coding has been going on behind the scenes as usual. A lot of that work has been centered around the picker.

In the STAGING buffer technique we were using, I knew that we were meant to pull the results on the following frame. However, I had been cheeky and tried to pull it on the Unity CommandBuffer event `AfterEverything`. This didn't work well, and I saw significant delays on the render thread; it makes sense and felt like something I would have to change.

We added support for pixel picking to the dice and made it play nice with hide volumes (so you can't pick things that should be invisible). While coding that I noticed that hide volumes were not applied correctly to pasted tiles or tiles created by redoing placing tiles. I chased this down to a simple case where we were not setting up a tiles state before trying to set apply the hide volumes to that state.

On the subject of picking: After a little experimentation, we switched from using the Depth buffer from the currently rendering scene to giving the pixel picker it's own depth buffer. This way, we could easily exclude things we didn't want to be pickable. We decided to try this after noticing the tile/prop/creature preview would be detected by the picker when placing them, as that would have been a bit annoying to work around.

The next task was simple but takes a little context to explain. The picker and physics raycasting code returns a reference to tiles/props, which is basically just a fancy pointer[1]. Tile & Prop data can be moved when a zone is modified so, if you want to store a reference to a tile/prop that is valid for multiple frames, we make a handle that has a safe way to lookup an asset at the expense of some performance. Previously you would create a new handle whenever you wanted to hold on to a tile/prop, but allocating this object means that the GC has to clean it up at some point[2]. To mitigate this, we turned the handle into a sort of container. You create it once and then push a tile/prop reference into it. You can then replace the reference in the handle or clear it. 

Then there were a bunch of physics-related tasks:
- A couple of bugs in our wrapper around `Unity.Physics` resulted in ray-casts hitting things that had already been destroyed. 
- Replace the mesh colliders around the creature base with a single CylinderCollider
- Dice has not been set up to interact with creatures
- Improving handling of framerate spikes
- My code for creating the cylinder geometry was off by 90degrees, so we ended up with stuff like this happening
![wat](/assets/images/cylinder.png)

> Note: It is super obvious what is happening in this picture, but when I noticed it, the only cylinder collider was around the creature's base, and so I thought I still was messing up creature/dice interactions. I had a good solid facepalm when I finally saw what was up.

The following item on the bug list was that shadows were not respecting hide volumes. This was harder than it should have been due to hiccups we'd had while rewriting the engine. Let's get through the basics:
- GameObjects were too slow
- Unity has calls to render objects using instancing without using GameObjects
- Those calls cause issues with shadows as Unity can't them per-light and so does way too much work
- Unity added a new thing called a BatchRendererGroup, which lets you make big batches of things to render and implement the culling yourself (Yay!)
- We implemented our new code based on the BatchRendererGroup
- What wasn't documented was that specifying per-instance data only works if you are using their new Scriptable-Rendering-Pipeline (ARGGGHH)
- We do not have the time to write our own rendering-pipeline and port all the shaders
- We decided to implement our own GPU culling and use indirect rendering to draw the board with shadowcasting turned off and keep using the BatchRendererGroup for shadows.[3]

This has worked well, but as we can't get per-instance state to the shadow-shaders, we can't use our usual approach to cull things in hide volumes. Instead, we now check the hide volume state when doing the CPU-side culling of shadows. This would require too many pointer lookups, so we added an extra job to copy the data into another buffer for fast lookup.

Dice 'sleep' was next on the agenda. When an object has stopped moving in most physics engines, the engine puts the object to sleep and stops simulating it. The new Unity.Physics library does not have support for sleep built-in as the library is intended as something to be built upon, and different sleep implementations have various tradeoffs. Instead, what we are doing is simply snapping the die to a stop when it's velocity and position has stayed at a low value for a given period of time. This stops the dice from drifting but doesn't stop the physics simulation. To prevent things from (computationally) getting out of hand, we are adding an auto-despawn of dice rolls once they have been at rest for a given duration. This also starts addressing some feature-requests about dice management, so that's nice.

One thing that is not resolved is that we are seeing inconsistent execution time for some operations related to picking. It's a bit confusing as it's the enqueuing of the task that is taking a long time rather than the task itself. I need to collect some more data and post a question on the Unity forums as I'm a bit lost as to why this is happening.

Amongst all this is the usual slew of small issues and fixes, but this is getting plenty long enough already.

I'm immediately switching tasks from front-end work to the per-zone sync that I've been talking about for months. The next updates will be all about that.

Until then :)


[1] We implement them as readonly ref structs, so we don't accidentally store them 
[2] GC really is the enemy in games. You might be surprised how badly timed it hurts performance even when using an incremental GC.
[3] Another approach would have been to write our own lighting/shadow system, but that's also terrifying given how late we already are with the Early Access :/

