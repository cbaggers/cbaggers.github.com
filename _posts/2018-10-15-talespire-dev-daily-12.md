---
layout: post
title: TaleSpire Dev Daily 12
description:
date: 2018-10-15 22:19:23
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today has been spent replacing the old fog of war with the new code. This code has paths from and through many other systems so it feels like I've been pulling on threads to see what moves and then unweaving them.. flashbacks to running network cables in an old job :p

Like yesterday I don't have solid stuff to show but it feels like it's getting close. Tomorrow will start with testing followed by hooking the level loading into the fog of war stuff. This either means having each loaded asset write add themselves to the zone's octree or (much more likely) we just serialize the octrees.

I also need to look into which zones are updated per frame. For a huge map you don't want some zone a mile away updating things that don't matter.

Thanks for stopping by, seeya next time
