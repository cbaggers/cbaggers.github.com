---
layout: post
title: TaleSpire Dev Log 71
description:
date: 2019-02-21 22:06:06
category:
tags: ['BouncyRock', 'TaleSpire']
---

Progress has been unexciting but continuous. I've pushed the changes that reveal corners of boards correctly in player mode. It does reveal a little too much but this will be fixed in future when the resolution of the 'fog of war' datastructure is increased.

Today I added the class for prop assets and refactored the building/serializing/etc code to handle this new asset kind. This being seperate now lets us at unique logic for these assets. We have experimented with having props temporarily 'dodge' out of the way of creatures and also be able to be places and rotated with more fine grain control than of tiles. Those experiments can now we ironed out and made part of the shipping game!

@Ree, whilst ill, has put a bunch of time into the changes we need to be able to more reliably get the invite emails passed filters. More on this soon.

I'm not going to be around on friday but will be working on Sunday instead so hopefully I'll have another post for you then.

Peace.
