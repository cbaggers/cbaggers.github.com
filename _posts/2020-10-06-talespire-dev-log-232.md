---
layout: post
title: TaleSpire Dev Log 232
description:
date: 2020-10-06 10:09:36
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Updates from the land of code.

The batching/rendering code has reached where we need it to for the Early Access. We see a three times speedup on some of our stress tests, and while there are very clear ways to get more, this endeavor has already left us two months behind schedule, so we need to press on into other features. That said, it is lovely to see all this work pay off, and I'm very excited by what we can do to get the rest of the perf that is still on the table.

During this, I hit one very annoying bug. It seems that Unity does not handle writing into IndirectArguments buffers properly on all GPUs[0]. The GPU-frustum-culling system I wrote recently worked great on my home PC but failed silently and strangely on my laptop. I ended up using a more naive approach and having the culling compute shaders pushing values into an append-buffer and using the CopyCount method to transfer the final length to the IndirectArguments buffer afterward. This still kept things on the GPU but significantly increased the number of compute shader dispatches and buffers required to work. The good news is that it does open up some opportunities for removing some indirection in the vertex shaders, which should give some performance wins later. I *think* I should also be able to make a system on top of this that can reduce the number of final draw calls, but we'll see how that goes.

Ree and I did some digging into the physics instabilities we have been seeing. Along with some simple mistakes, we remembered that the Unity.Physics API allows you to swap out the engine used behind the scenes, and that Havok has such a package. We had heard rumors that it was just plug-and-play, but when is programming ever that simple?

It was that simple. To be honest, I was floored. I changed maybe ten lines of code, and TaleSpire was using Havok. We saw that, while the stability was a little better, we were getting uncannily similar issues to using Unity's DotsPhysics. This made us confident that the problem was how my code is driving the physics rather than the instability coming from the engine.

It should be noted that DotsPhysics was significantly faster than Havok in the stress test we threw at it. TaleSpire doesn't have many dynamic objects, but we have hundreds of thousands of small static objects. DotsPhysics seems to be heading a very compelling direction, and I think it will carve out a very successful niche for itself.

We then upgraded to the latest version of Unity, which, in turn, allowed us to move to the latest builds of various packages, including physics. The improvements in DotsPhysics was again, notable. It seems that our strange blend of using old and new systems in Unity has paid off well as we haven't yet run into any painful bugs on this new build. Unless anything major happens to make us have to downgrade, 2020.2 will be the version of Unity we will ship TaleSpire using.

We also merged master into the branch with all the new tech. This includes the props system. To let prop development be done in parallel, Ree has been serializing the props separately from the tile data. Now that props are shaping up well, we can see what data is needed and so we are in a good place to bring props into the same system as tiles. This will let sync, undo/redo, and all the other normal stuff work with props too.

A short time back, discussions with the community on how line-of-sight should work led me to re-think how that system should be implemented. Yesterday, Ree helped me prototype one possible version of this. It relied on us not using the BatchRendererGroup in ShadowsOnly mode but instead discarding fragments in passes where we didn't want to write to depth. It worked well in small scenes, but we took a significant FPS hit in our stress-test. I'm happy that we found this out very quickly and can now start the version that is a little more work but will not put additional costs on unrelated systems. That version will require us to dispatch the DrawMeshInstanced calls to render the scene to individual faces of the cubemap. We will use jobs to handle the selection & per-face culling of the tiles for these draw-calls.

Aside from this, I've continued work on the character controller. It's not there yet, but we are relatively confident we can fix up the remaining issues, so it's currently a lower priority.

That wraps up everything for last Wednesday to now. My next immediate task is to hunt down what I'm doing wrong in Physics as currently, it's the biggest thing that stops us from play-testing this branch.

Hope you folks are doing good.
Peace.


[0] It could be that the IndirectArguments issue was a graphics API limitation rather than a Unity one. I'd need to do more reading to find out.
