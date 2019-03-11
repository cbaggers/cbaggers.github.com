---
layout: post
title: TaleSpire Dev Log 79
description:
date: 2019-03-11 22:35:31
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today has been fun. Hacking together small features feels super rewarding after being down in the weeds for a while.

Today I got size indicators working for multi-select and dragging out tiles. The multi select version needs a little work as sometimes the rounding produces slightly unintuitive values, but that should be a quick fix so you should have this in your hands in the midweek update

<video controls src="/assets/videos/sizeIndicator0.mp4" type="video/mp4" width="620">

Next I started work on another community requested feature, torches for creatures.

<video controls src="/assets/videos/creatureTorch0.mp4" type="video/mp4" width="620">

This one still needs a few tweaks so I'm expecting this will be in the weekend update.

@Ree has been hard at work refactoring how we handle animations for creatures so you will be seeing some new stuff there soon. The first thing we expect to bring is creature death animations. This will also leave the 'corpse' in the scene so you can revive it if you like.

The reason for the revamp of the animation stuff is that it allows us to more easily add other *things* into the timelines, like sound & visual effects (think sexier magic attacks!) so it's opening up some exciting stuff.

Seeya tomorrow
