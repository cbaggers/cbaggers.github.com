---
layout: post
title: TaleSpire Dev Log 250
description:
date: 2020-12-24 16:47:52
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, I've got a beer in hand, and the kitchen is full of the smell of pinnekjøtt, so now feels like an excellent time to write up work from the days before Christmas.

This last week my focus has been on moving (still) and props.

As mentioned before, I've been slowly moving things to my new place, but things like the sofa and bed wouldn't fit in our little car, so the 18th was the day to move those. I rented a van, and we hoofed all the big stuff to the new place. The old flat is still where I'm coding from as we don't have our own internet connection at the new place yet, so I'm traveling back and forth a bunch.

Anyhoo you are here for news on the game, and the props stuff has been going well. Ree merged all his experiments onto the main dev branch, and I've been hooking it into the board format. The experiments showed that the only big change to the per-placeable[0] data is how we handle rotation. For tiles, we only needed four points of rotation, but props need 24. We tested free rotation, but it didn't feel as good a rotating in 15-degree steps.

The good news is that we had been using a byte for the rotation even before, so we had plenty of room for the new approach. We use 5 bits for rotation and have the other 3 bits available for flags.

I also wanted to store whether the placeable was a tile or prop in the board data as we need this when batching. Looking up the asset each time seemed wasteful. We don't need this per tile, so we added it to the layouts. We again use part of a byte field and leave the remaining bits for flags. [1]

There are a few cases we need to care of:

1. Tiles and Props having different origins
2. Pasting slabs that contain placeables which the user does not have
3. Changes to the size of the tiles or props

Let's take these in order.

### 1 - Tiles and Props having different origins

All tiles use the bottom left corner as their origin. Props use their center of rotation. The board representation stores the AABB for the placeable, and so, when batching, we need to transform Tiles and Props differently. 

### 2 - Pasting slabs that contain placeables which the user does not have

This is more likely to happen in the future when modding is prevalent, but we want to be able to handle this case somewhat gracefully. As the AABB depends on the kind of placeable, we need to fixup the AABBs once we have the correct asset pack. We do this in a job on the load of the board.

### 3 - Changes to the size of the tiles or props

This is a similar problem to #2. We need to handle changes to the tiles/props and do something reasonable when loading the boards. This one is something I'm still musing over.

### Progress

With a couple of days of work, I started being able to place props and batch them correctly.

I spotted a bug in the colliders of some static placeables. I tracked down a mistake to TaleWeaver.

With static props looking like they were going in a great direction, I moved over to set up all doors, chests, hatches, etc., with the new Spaghet scripts. This took a while as the TaleWeaver script editor is in a shockingly buggy state right now. However, I was able to get them fixed up and back into TaleSpire. I saw that some of the items have some layout issues, so I guess I have more cases miscalculating the orientation. I'll look at that after Christmas.

In all, this is going very well. It was a real joy to see how quickly all of Ree's prop work could be hooked up to the existing system. 

After the break, I will start by getting copy/paste to work with props, and then hopefully, we'll just need to clear up some bugs and UI before it's ready to be tested.

Hope you all have a lovely break. 

God Jul, Merry Christmas, and peace to the lot of ya!



[0] A 'placeable' is a tile or a prop
[1] I'm going to review this later as it may be that we need to look up the placeable's data during batching anyway and so storing this doesn't speed up anything.
