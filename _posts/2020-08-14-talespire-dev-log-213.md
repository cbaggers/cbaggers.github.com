---
layout: post
title: TaleSpire Dev Log 213
description:
date: 2020-08-14 08:45:48
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Morning all,

Yesterday I got the new collider data loaded into TaleSpire and started writing the code that would lay out all the static colliders.

I originally planned to store the colliders per asset[0], but after writing a significant amount of code, I remembered that Unity.Physics' colliders don't support scaling. This means I need to store scald versions of the asset's collider for each Tile/Prop[1] that uses it.

I realized this fairly late in the day, and it was a bit of a bummer to have to start rewriting then, so I stopped for the evening and relaxed instead.

Today I've got to detour into some other code, but I'll get back to this asap.

We do learn from this however, that we won't be able to animate the scale of the parts of assets that have colliders. I'm not sure if this means we'll just disallow animating scale or only give warnings if you do it to something with scale. I'll chat with Ree about this in our next daily.

Alright folks, hope you are doing well.

Seeya


[0] In TaleSpire, Tiles, Props, and Creatures are made of assets arranged in different ways. Assets themselves are made of objects, but you don't address those sub-assets directly.

[1] Each kind of tile, not one for every instance of the tile.
