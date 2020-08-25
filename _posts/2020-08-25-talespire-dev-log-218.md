---
layout: post
title: TaleSpire Dev Log 218
description:
date: 2020-08-25 17:53:43
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Today I started out looking at the slab code again. The slab is the way we refer to a section fo tiles that have been copied. When you paste a slab string, that slab appears in your 'hand' in-game, and you can place it like a tile.

Recently we have been writing batching code, something that used to be handled by Unity. It got to the point where I needed something similar for the slabs, which I started yesterday. There are a couple of differences from the standard code batching code:

- We don't want physics to be set up for the tiles/props
- We don't want scripts to start running
- We need to update the position of every object in the slab every frame (as the player moves it)

This mainly involved taking the existing code, taking a chainsaw to the parts we didn't need, and stitching together the remains. It's still very rough, but I was able to try pasting in the slabs from the community... which of course crashed the game :P It was obviously not the fault of the community content, it was that it was a much more complete test that I had run up until then and it found a few bugs. One was a case where we had assets in TaleWeaver with a [MeshFilter](https://docs.unity3d.com/2017.3/Documentation/Manual/class-MeshFilter.html) with no mesh specified. Another was that I hadn't written the code to handle missing assets.

I got the code to the point that I could paste a slab. However, it feels pretty rough as the ground plane in the game doesn't exist in the new physics system yet. I'll be back working on the physics code soon, and then I can take another crack at this.

The rest of today has been spent looking into building code. I need to re-implement the stuff that handles things that have been affected by changes that have not yet been committed (confirmed by the host). It's pretty hard going, but I'm hopeful I can make some decent progress this week.

I'll leave ya with a view from the cabin one of our friends were kind enough to invite us along to over the weekend,

Ciao

![the hills](/assets/images/trip0.jpg)
