---
layout: post
title: TaleSpire Dev Log 215
description:
date: 2020-08-17 09:19:38
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Progress feels slow but is steady.

The batcher now produces the data the physics engine needs for static colliders. Thanks to that, I've started writing some helper methods that mirror the parts of the older physics API we use. This should make the porting of the existing code noticeably easier. I'll find out how that goes today as I'm planning to start porting that now.

You'll notice I specifically mentioned static colliders above. I haven't worried about the colliders attached to objects moved by scripts yet. The reason is that they are, for the most part, just a slightly more complicated version of the thing I've already done, whereas the board tools and dice are still unknown quantities.

I'm hoping the tool side goes smoothly today as I really want to start looking into things like dice and creature, which will have a hybrid setup in this system. By hybrid, in this case, we mean that we will be using Unity's GameObjects, but we will write custom components to hook them into our physics setup [0].

Speaking of the physics setup, we've always had two sets of colliders we keep in different layers in Unity. We have the 'explorer' layer, whos colliders we use when placing tiles, and the 'tile' layer[1], which is the version used during play by dice/creatures etc. The split allows us (and modders) to provide a better building experience by tuning the explorer colliders. As I've been working with the new physics engine, I've started to think that it might be worth using two totally separate physics-worlds, rather than layers.

The reason is one of performance. When querying the colliders, we usually care about either the explorer layer, or the tile one and the physics engine has to skip the ones we don't care about. It does this via a layer mask, so it's a quick check, but not doing something is always faster than doing something. I'm also musing about if this allows us to do less work per frame by having different update frequencies for the two worlds. We will be trying to keep things fast by, per-frame, only giving the physics engine data for the zones[2], which contain the player's cursor or dynamic objects like dice. However, there is a tradeoff between changing data (which requires more frequent rebuilds of internal data-structures[3]), and the number of things being tracked by the physics engine (which affects query times).

This is something that can be tuned over time, so I may just set up the code so that swapping and changing will be less painful in the future.

Alright, that's all that I need to ramble about for now. Hope to see you in the next one.

Ciao.

[0] In contrast, all tiles/props are managed/batched by us. It's a tradeoff between having Unity help us with lots of things and performance.

[1] Yeah, not great naming :P

[2] 16x16x16 regions of world space

[3] BVH https://en.wikipedia.org/wiki/Bounding_volume_hierarchy



p.s. oh yeah I've also started on tools to visualize colliders in our new system

![raycast](/assets/videos/cast.gif)
