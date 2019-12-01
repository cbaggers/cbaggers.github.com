---
layout: post
title: TaleSpire Dev Log 134
description:
date: 2019-12-01 18:41:55
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Alright, part 2!

So in the last post, we talked about updating the data representation of the board. This time let's get into the bit the players see and interact with. The GameObjects.

In Unity's currently supported approach, any *thing* in the world is a GameObject[0]. GameObjects do very little on their own but their properties and behavior come from Components.[1]

We don't need to understand much more about this for now except that Unity naturally has to manage all these GameObjects and, as with much of the API, you can only interact with them from the main thread.[2]

### Rendering

One of the nice things Unity does do for us is handling instancing so that it's fast to draw many of the same kind of object. For example, if we have 1000 of the same tile in the scene then Unity can make one draw call to the GPU to draw them all.[3]

We aren't going to talk about rendering any more in this post but we will revisit it in the next one as there are some issues we need to overcome.

### Spawning

So we have a TileSpire-like game where we want to have the players be able to make tonnes of tiles, they should be able to drag out or paste big slabs and the delete individual tiles as they please.

If someone drags out a 30x30 slab, we need to spawn 900 tiles. Depending on the complexity of the tile Unity might not be able to handle instantiating that many new GameObjects in a single frame. This is serious though as when people are building we have building actions arriving from other players fairly frequently and we wouldn't be able to apply the changes robustly until all the GameObjects are made.

The first change we could make is to keep the data for Tiles separate from their GameObjects. This also gives us opportunities to store that data in [blittable](https://docs.microsoft.com/en-us/dotnet/framework/interop/blittable-and-non-blittable-types) types and use Unity's Job system to operate on them (like we talked about in the [last post](https://bouncyrock.com/news/articles/talespire-dev-log-133))

> I'm going to shift from talking explicitly about tile data and GameObjects to talking about the tile data and it's 'presentation' unless the fact that GameObjects are involved is important

With this, we have the opportunity to make the data changes immediately and apply them over a number of frames. This is the approach we are using and is a part of why we want to apply data changes as fast as possible, because there may be multiple arriving in a single frame.

### Relationship management

One issue we have given ourselves is that, now that the data and presentation are separate, we need to keep then in sync. If someone deletes a tile we need to delete the presentation too and, naturally, the same goes for undo/redo/etc. We could keep an index from the data to the representation but this has implications:

- We need an extra integer of storage for every tile. 4 bytes is nothing on its own, but 100000 tiles is not a crazy number of tiles to expect to have to deal with so it does add up.
- It may benefit us to have different layouts for the data and the presentation arrays. That could mean having to update the data->presentation index. One can then assume you'll need to have a way to do this quickly.

Now you may not care about either of those, which is valid too. But whichever way you pick there will likely be tradeoffs.

One simplification we can do is to note that tile data is added in chunks. If we keep presentations of tiles from the same chunk together then you only need indices per chunk, this can help in the approach you pick.

### Avoiding work

Other than being able to spread out work over multiple frames we get some trivial opportunities to avoid work too.

First off, if we can apply changes to data first and then generate the changes to the presentation afterward then we often get to skip work. Imagine an Add followed by a Delete arriving in the same frame, by looking at the data afterward we only need to spawn the tiles that remain, rather than first spawning them all and then applying the delete.

Also in the case where there is a small backlog you can get opportunities to skip entire actions. The best case is an Undo & Redo together (this can happen when people are playing around to see if they like a particular change). In that case, you can remove both actions from the queue as they cancel each other out.

### You don't always need to present anyway

If you have players in two different parts of the board you may want the data for the zones in memory but not have to have a presentation of the Zone you are nowhere near. This is trivial here as the data is separate.

This makes jumping to that part of the board much less jarring than it would be as you are already to date with the latest version of that part of the board.

### Applying changes

A Zone's presentation now gets given a 'budget' of how much it can change this frame. It keeps popping changes from its queue and applies as much as it can until the budget runs out. The budget is represented by a floating-point number and we give separate costs to spawning tiles, reusing ones from the GameObject pool, deleting tiles, etc.

These numbers are made up by us but they are easily tweakable so we have a lot of freedom now to profile and find out what works. We'll chat more about this in the future when we get more data.

### Sticking it together

So we are doing a bunch of these things. We have a separate presentation for each Zone. They each have queues of changes to apply which, due to how tiles are managed in data, mainly boils down to adding and deleting chunks of GameObjects by their Id.

We use the code that applies the change to data to make simpler change instructions for the presentation so that it has much less work to do.

We don't maintain any indexes between the data and presentation (beyond unique ids of chunks) so we don't have to do any upkeep work, but we do end up scanning tiles in more cases that we would have to otherwise [4]

We don't do all the work avoiding stuff yet but the hooks are in the code so we can add this easily when we want to. First off I need to iron out the bugs that remain.

### Next post

Ok so that wraps up this post, in the next one, we need to talk a little about rendering and a few miscellaneous bits and bobs we've also done this past week.

Ciao

[0] Ignoring drawmeshinstanced and friends for this as it makes for an easier to parse, if slightly inaccurate, sentence.

[1] At least, only the Unity data, you can change the fields on your components as you like.

[2] I should also mention we are not using the new ECS yet as some features we need are only just arriving and we would still need to do a bunch of work to test everything we need will work in the new system. I'm 95% sure we'll be moving to it within the next year though.

[3] Because of reason 1023 is the largest number of instances that can be made in one call in Unity. This is a limitation of their system and it's a bit of a shame, but we will deal with this another time

[4] Tiles in the presentation are still currently in the same order as the data representation, so this does still leave us with ways of making applying deletes less costly. Currently, this is not an issue though so I'm leaving it for now
