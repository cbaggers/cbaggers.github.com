---
layout: post
title: TaleSpire Dev Log 162
description:
date: 2020-03-04 10:02:35
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Howdy folks,

Alright, so yesterday I working on a few bugs in the code that handles the Unity GameObjects of the tiles (I'll be referring to this as the 'presentation' as opposed the data-model which is separate).

I did a little work, so that undo/redo reuses portions of the `List` that holds the game objects. It wasn't too tricky, but as the presentation adds/removes assets progressively over several frames, I had to take that into account.

I fixed a bug where rotated slabs weren't be pasted correctly as it was considering the wrong zones for the paste. This was simply because, when considering what zones to paste to, I hadn't rotated the bounds of the slab.

I fixed a few bugs relating to how, when we have updated the state of a script, we locate the asset so we can update the visuals. In the end, I've opted to have a per zone presentation hashmap. This is a bit annoying as I wanted to avoid yet maintaining another data structure if we could instead navigate to the asset another way (even if it was a bit less efficient); however, this will get us out the door.

Fixed a bug where progressive loading was skipping operations that it was meant to apply to the presentation. This was a dumb mistake of just spuriously advancing through the queue in two places. Probably a screw up during refactoring.

Added a bunch of asserts performing sanity checks on the data-model. These are only included in the debug build, so I get crashes nearer the cause of the issue, rather than always having to infer what screwed up.

There was also other stuff, but it gets too into the weeds to cover in a daily dev-log.

Today I'll be going through a tonne of the code handling the network connections and board synchronization and trying to add sensible behaviors to the failure cases. For example, when we join and try to pull the board from another player:

- what if they aren't ready to sync
- what if they are already uploading the board
- what if they start, but then that fails
- what if they never receive the message to sync.
- etc

It needn't be too fancy, but we need to have some logic dictating what to do, how many times to retry, and so on.

That's all for now,
Seeya
