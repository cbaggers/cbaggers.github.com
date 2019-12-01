---
layout: post
title: TaleSpire Dev Log 133
description:
date: 2019-12-01 16:41:10
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks,

There have been no daily updates for the last six days as I've been in a sort of self-imposed crunch to hammer out some things that have been on my mind.

The first one was related to the performance of Add/Delete/Undo/Redo operations on tiles. The faster we do this the better and we have a bunch of opportunities to make this quick. The first was to move the core code of these features over to Unity's [Job System](https://docs.unity3d.com/2018.3/Documentation/Manual/JobSystemOverview.html).

## Why we really have to care about performance

For newer arrivals to these writeups, performance may seem an odd concern for a game where the rate things are happening seems much lower than that of, for example, an FPS. For us, it's not the frequency of actions that plague us but the volume of work that can be created by them, and the fact there is no general pattern to when they happen.

For example, dragging out a 3x3 square creates 9 tiles, but 30x30 is 900 tiles.

- Tiles are usually made of smaller assets so that's 900*N object to spawn[0] and render
- If any of those sub-objects have scripts then that's 900 more scripts running per interactable sub-object
- The first thing tiles do is drop into place, so you best be ready to animate 900 of those (we do this via shaders)
- When people drag to select we slightly raise the tile to show it has been selected so you will need to be able to query many thousands of tile bounds quickly.
- etc

.. and 30x30 tiles isn't many considering a board is meant to be 30000x30000 units wide and 10000 units high.

All of this isn't to complain, we want the game to behave like this, but we do need to be clear to ourselves on what needs to be achieved within the 16ms of a frame[1]

## Jobs

So back to the jobs. One of the first things I looked at was the collections we use to store the tile data. As of today, the data for tiles are split up somewhat like this:

- Board: contains N zones
 - Zone: contains up to 16 client-data-chunks, one for each client[2] who has built in a zone
  - ClientDataChunk
   - AssetData: Holds the data for the tiles themselves
   - AssetLayouts: Holes layout information of the data in AssetData

As you can imagine we have lots of opportunities for performing operations in parallel here. Zones are a 16x16x16 unit chunk of the board and so if an add or delete crosses multiple zones all of those adds/deletes can potentially be done in parallel.

Also as each client's data is separate within the zone we have opportunities here too. This has so far been less useful as an Add only affects the data of the client that performed it, and during deletes, we want to store the tiles that were deleted for undo/redo and so that would require collections that concurrently have arbitrary numbers of tiles written into them. Making a new collection type to cover this case was too much of an additional distraction and the common case is one or two people building so there is less parallelism to be gained here anyway. [3]

## A diversion into better collections

Here is a rough idea of how AssetData is laid out.
```
// (the data is actual flat but grouped here for clarity)
AssetData: [ [Tiles from Add 0] [Tiles from Add 1] [Tiles from Add 2] [Tiles from Add 3] ]

ActiveHead = 4
Length = 4
```
We tiles we consider active are all from the Add less than ActiveHead. So in the above case, all of them

One of the pain points from the alpha was Undo/Redo feeling unresponsive with large numbers of tiles. With this layout, the undo of an Add is just subtracting 1 from ActiveHead (we will talk about spawning the visuals for the tiles later). By not deleting the data yet Redo is just adding 1 to ActiveHead.

If you Undo and Add and then perform an add or delete action there is no way to Redo that add so we just overwrite its data with new tile data. Thus we rarely *need* to shrink the backing NativeList behind AssetData.

Now having the Head integer and Native collection separate is fine but as they need to be updated from jobs together it helps for them to be together in a native collection that properly uses Unity's thread safety checks to make sure nothing funky is going on. To that end, I spent a little while making a collection with the terrible name NativeGreedyList.

It has a Capacity, and ActiveLength and a FullLength. The FullLength is the apparent length of the List. The ActiveLength is the ActiveHead integer from before. Capacity and FullLength are separate as you often want to allocate in larger increments than you immediately need so you have to resize less often.

Unlike NativeList we don't every reduce the capacity unless explicitly required to (as the data past the ActiveLength is often still valuable to us)

## Back to jobs and the problem with multiple people building

Alright, so armed with new collections the work Jobifying everything continued. After a bunch of wrestling and learning, I got this into shape and so now the data updates are parallel. Yay!

But of course, there is more.

One thing that has been on my mind is 'roll back' and 'reapply' and that what I'm going to ramble about now.

When you have multiple people changing the board you need to make sure that each change is applied the same on every client to make sure they all end up with the same board[4]. When you only add stuff it's easy but with Delete the result depends on the order the operations are applied.

To set the order each 'action' is sent to one client who is deemed the 'host' and they give it a history-id and send it on to the rest of the clients. Great, this gives everything a set order but it also means that you don't see a change until it's made that round trip to the host. That kind of delay is unacceptable (it feels awful) so we did the following:

- Send the action to the host
- Apply the change locally immediately
- *time passes*
- An action arrives from the host
  - if the action is the change we already made, we don't need to do anything.
  - if the action is from a different client we:
    - 'roll back' our change
    - apply the action from the other client
    - 'reapply' our change

This works but necessitates some form of 'snapshot' we can roll back to[5]. This has been a pain for a while and something I've never been happy with. However, during the week I was looking at the problem of spawning the Unity GameObjects for all these tiles (more on that in the next post) and I spotted a cool bunch of details.

> For the rest of the article we will use the term 'uncommited' to refer to actions/tiles we have made but that have not yet been confirmed by the host, and so would have to be rolled back

- Each client has separate collections for their assets in a zone
- Adds only affect the data for the sending client
- Deletes can affect data for all clients
- Deletes have not only a region in space but also a point-in-history (as a selection may be made before the person hits the delete key)
- An uncommitted change, by definition, can only be on the client that made it

So by making sure that delete only operates on tiles that have been committed then we don't need to rollback for this case. We do have to reapply our deletes though as they may have added tiles one of our selections should have captured. When we reapply a delete we only do it to the data of the clients whose actions were committed before ours.

Undo & redo are also simple as they can only affect asset data for the client that performed the action[6]

With all this in place, we get to totally remove snapshots. It requires a bunch of tweaks to how changes coming from the network are applied.

This is one of those lovely times where data layout fundamentally change what has to be done to implement a feature. I find that stuff pretty cool.

## MORE!

This post has got long enough but there is still plenty more to cover from this week so let's get back to it in the next post.

Ciao

[0] We do use instancing but until Unity's ECS supports per instanced data we are forced to stick with the classic GameObjects approach which means we do need to spawn separate GameObjects.

[1] or, of course, spread out across many frames

[2] A client, in this case, is a single running instance of the TaleSpire executable.

[3] This will be revisited in the future as that collection type would be very handy. I also didn't use NativeQueue as ParallelWriter is only for use in IJobParallelFor jobs as far as I can tell.

[4] Previously we have talked about how we can't afford to be sending all the tile data across the network so instead, we just send the building 'actions' the players perform.

[5] Using Undo/Redo wouldn't work in this case due to how deleting tiles placed by other clients works[6]

[6] If you delete tiles from another client and then Undo that delete the tiles are not placed back in their collection. Otherwise, if they undo and then redo they restore those tiles, which feels a bit odd.
