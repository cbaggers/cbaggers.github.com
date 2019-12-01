---
layout: post
title: TaleSpire Dev Log 135
description:
date: 2019-12-01 20:23:16
category:
tags: ['Bouncyrock', 'TaleSpire']
---

*Sound of trumpets* BEHOLD, Part 3 commences.

We have talked about updating the data and the presentation but we still have to animate the tiles.

In TaleSpire we want to be able to have animate tiles as they appear and disappear as it makes the game feel better. We have a lot of tiles to update so we have to take care of how we do this.

## A simple problem

We wouldn't want to have to update the positions of the GameObjects every frame so we animate the drop-in using the vertex shader. The animation curves are simply stored as ramps inside textures we can sample over time. This means we only have to update the per-instance data for the GameObjects when we want to change which animation they are running.

## A secondary problem

One annoying thing though is that, when changes to this state do come, they tend to come in large numbers. One example of this is during tile selection.

When players select tiles we raise them very slightly so show that they have been selected. This means we need to update the per-instance data for those tiles. Also if the tiles have props attached to them those props need to be raised too.

Unity's interface to this is via [MaterialPropertyBlock](https://docs.unity3d.com/ScriptReference/MaterialPropertyBlock.html)s and like so many things in Unity it can only be updated from the main thread. This kinda sucks as what we need to do is perfectly parallelizable

> If the tile bounds intersect the selection bounds then set the value

This is extra annoying when we consider that selections bounds are 3D and so the number of tiles that may be involved goes up terrifyingly fast.

To resurrect our example from the previous articles a 30x30 tile slab contains 900 tiles. Due to TaleSpire's smallest tile size being 1x0.25x1 units A 30x30x30unit region could (theoretically) contain over 80000 tiles.

Obvious that is insane and we will need to add some form of density limit, but the game will need to communicate this limit to the players in a way that feels fair.

Once again user-generated-content games bring their own flavor of crazy to the party :)

## The secondary problem with a hat on

But we aren't done, oh no we are not. Last time we talked about progressively applying changes to the presentation. Due to this, a selection can be made on tiles that are still spawning. According to the data layer, you selected perfectly valid tiles but the presentation hadn't quite caught up. Maybe the tile spawned the frame after you let go of the selection and so as far as you percieved it was there when you let go.

Now there are different ways to resolve this but the way I want to go with for now is that the data is the source of truth, if you selected the right region you selected the tiles even if the presentation hadn't quite got there yet.

I expect this to be an edge case that people won't even notice, but it is better to have an answer for it.

## One change

So let's talk about one change that helps here. What we are going to do is move the tile's animation state out of its per-instance data and into a separate [buffer on the gpu](https://docs.unity3d.com/ScriptReference/ComputeBuffer.html) (let's call this the tile-state-buffer). Then we will put the index into that buffer into the tile's per-instance data instead.

This little change lets us solve all the above issues.

First off we will have a copy of the tile-state-buffer in local memory. This will be a [native container](https://docs.unity3d.com/Manual/JobSystemNativeContainer.html) so that we can operate on it directly from Jobs. This means all those selection updates are done in parallel now.

Next, we just gained the option to push only part of the local buffer to the GPU per frame. This is super handy in cases where huge numbers of tiles are being changed per frame. There is a concern this delay to the start of the animation could feel slightly unresponsive, however, I'm expecting that as long as *something* starts happening on the same frame as the action it should feel ok. Especially as this means that lots of changes are being made.

This also plays nicely with our selection -v- progressive update issue. We can now make the changes to the tile-state-buffer independent of whether the presentation for the tile has finished loading yet. As soon as the GameObjects are spawned they will put their state index into their MaterialPropertyBlock and they will be up to date immediately.

## And another thing

One obvious this I skipped when presenting the selection problem is that updating all tiles in the selection is totally unnecessary. We only actually need to update the shader state for the tile that has become selected or unselected since the last frame.

From the following diagram, we can see that in 2d that means you are inside one of two rectangles. For 3D it's 3 cuboids.
```
                     frame 1                     difference
                  +----------------+    +------------+---+
                  |                |    |                |
   frame 0        |                |    |     0          |
+------------+    |                |    +------------+---+
|            |    |                |    |            |   |
|            |    |                |    |            |   |
|            |    |                |    |            |   |
|            |    |                |    |            | 2 |
|            |    |                |    |            |   |
|            |    |                |    |            |   |
|            |    |                |    |            |   |
+------------+    +----------------+    +------------+---+

```

Given that we can control how fast the panning in the game is this makes the number of tiles to update per frame much less. I left this until now though as selection makes a great example of the issue of tile counts in volumes and we have other operations for volumes that benefit from these changes too.

## More? More.

Well, this got long. I think I'm gonna save the miscellaneous stuff for another post.

See you there!
