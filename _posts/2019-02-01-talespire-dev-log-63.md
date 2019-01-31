---
layout: post
title: TaleSpire Dev Log 63
description:
date: 2019-02-01 00:07:12
category:
tags: ['BouncyRock', 'TaleSpire']
---

> WARNING: We want to be open in these technical blogs so you are reading unfinished, unproven ideas. This should not be taken as a promise of anything.

Today @Ree and I have been white-boarding a replacement for the floor system. We need to do this as the current system has a number of issues including (but not limited to):

- Tiles are revealed by floor which means large features need to be built on multiple floors or built in a huge stack from one floor which intersects the others
- building raised platforms that belong to one floor requires stacking and then deleting tiles as tiles always try to drop to the floor.
- hair loss
- Can't delete or position floors as the floor is infinitely wide. If you have a town and delete a floor it deletes that floor for every building in the town.

There are also a bunch of other things that fall out of this regarding how fog of war works, which is another area where the current version of the game has issues

We are making progress but it's a very extensive change. The goal is to let you create floor plains that are separate from each other. You will be able to adjust their height from the previous floor (in fixed increments) and they will grow as you place more tiles. The footprint is maintained down the whole stack of floors. 

This would let you have buildings where you can set floor height, delete floors, etc. Building raised platforms is easy and you just build the plain and build on top of it. 

Tile reveal and fog of war is also not ideal in open areas like towns right now. You build lovely multi story buildings and don't see them until you switch floor. Also, as we reveal what your character can walk to, players don't see roofs often either. We are musing on inverting the logic of fog of war, where you place a origin for the fog (let's call it a fog-machine for now :D) and then scroll (or something) to adjust how far the fog flows. this gives the GM control over what is shown and the same (or similar) method we have now is used to uncover it. Each 'body' of fog is separate so it should hug walls in a better way improving on the current tile reveal.. we also need some more shader magic to improve the look too.

So yup, massive changes could be afoot. This is ALL just thoughts and musings right now. We are going to start prototyping the gameplay side of floor building tonight and I'm musing on ways to maintain some of the nicer qualities of the fog of war whilst getting some of the new stuff.

I expect I'll do a stream soon to talk about the problem and the design space even if we have nothing new to show. We'll keep you posted with things as they happen.

Peace!
