---
layout: post
title: TaleSpire Dev Log 285
description:
date: 2021-07-13 22:13:56
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Here is a fun little tweak that is coming in an update soon.

<iframe width="600" height="450" src="http://www.youtube.com/embed/V5RarAY0Za8" frameborder="0">  </iframe>

In TaleSpire, we love the tile-based approach to things. We also love what can be achieved with clipping but, due to tiles have similar sizes, it's easy to cause z-fighting.

Yesterday, Ree suggested I try to add a tiny offset to the positions, so they were less likely to line up. But there is a caveat... we want the offset for a given tile to be the same every time you load. The reason for that is that it would suck if each time you joined the board, things looked very slightly different.

Tiles and props don't have unique IDs (as that would be way too much data), and we can't use their index in the arrays (as that changes when boards are modified), but we do have their position.

The position isn't necessarily unique, though, but when batching, we also know the UUID that identifies the 'kind' of thing being batched. This is ideal, so we mix some bits from the id, some bits from the position, feed it through a bodged noise function, and scale the result waaaay down. 

This tiny offset is stable and is enough to improve z-fighting in a bunch of cases. 

The eagle-eyed among you will notice that this doesn't fix cases where two of the same kind of asset exactly overlap. This is correct and is not something we are looking to improve as it's not a useful tile configuration anyway. 

So that's that. It's a blast to occasionally get these little things where a ten minute experiment can give such a cool result.

Aside from this, I've also pushed a patch to the database, which allows us to store data for a bunch of upcoming creature features (polymorph and persistent emotes among them).

Hope you are having a good day.

Peace.
