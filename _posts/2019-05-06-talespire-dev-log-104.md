---
layout: post
title: TaleSpire Dev Log 104
description:
date: 2019-05-06 22:56:35
category:
tags: ['BouncyRock', 'TaleSpire']
---

Allo everyone,

Today we made progress in a couple of areas.

I focused on changing how we prepare assets for TaleSpire. This is done in a set of tool in unity we call TaleWeaver. The new prop system allows you to clip props to other tiles at certain 'attachment points' in TaleWeaver you can now lay out these attachment points and set what the default prop for that point is. This means we can have pretty pre-made tiles that you can disconnect things from, which lets us hit that balance between providing player control and lets people just throw things together.

We now need to add attachment points to all tiles and, since that is quite a task, I'm looking at automating a bunch of it. That will be my task tomorrow.

@Ree has been working on camera control systems for the kickstarter trailer and added an adjustable radial-menu item that can be used for controlling stats. We've just hooked it up for HP and it seems to work well so we will provide HP and 4 other stats to start with. These stats are of course sync'd during the session and persist across the whole campaign if you make a creature unique.

The goal for midweek is for you to get copy/paste and the stats stuff so it's gonna be a fun one!

Seeya tomorrow!
