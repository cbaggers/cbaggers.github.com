---
layout: post
title: TaleSpire Dev Log 100
description:
date: 2019-04-25 00:10:54
category:
tags: ['BouncyRock', 'TaleSpire']
---

Heya folks, it's the 100th log so whilst I haven't got any special dev news I'd just like to thank all of you for checking out these little updates and TaleSpire as a whole. I could easily wax lyrical on how much your feedback means and how it has helped us work out what TaleSpire needs to be, but I'll spare you that for now. Regardless, Thank-You!


Now back to the snooker,

Today I've been working on the prop system. I've pushed the fact we are prepping large system refactoring out of my mind and am hacking in a version of prop support for the alpha.

The first version will go something like this:

- Tiles will stay pretty much how they are, snapping to the tile grid and rotating in 90 degree increments
- Props will be a special kind of asset which can be snapped onto attachment points on tiles
- Tiles will have many attachment points. These (for this version) will be authored in TaleWeaver when making the tile asset.
- Props will not obstruct line of sight or effect fog of war
- Placing a creature in the same space as a prop will cause the prop to temporarily 'dodge' out of the way (springing back afterwards)
- Props will be rotatable in finer steps (currently things 22 degree steps so 16 per revolution)
- Props rotate around the normal to the surface the attachment is on (really it's that they rotate around a vector defined by the attachment but the common case is normal of surface)
- Props will become children of tiles in the scene so will show and hide with whichever one they are attached to

The code I'm writing for this is real ugly but I'm just gritting my teeth and plowing on as getting a playable version will allow play testing which is way more valuable than holding of until after all the other things I want to change.

As this is a slightly larger task we aren't putting out a midweek update so we'll have to see what we have for the weekend.

Seeya around
