---
layout: post
title: TaleSpire Dev Daily 14
description:
date: 2018-10-17 17:24:50
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today I got a bunch of thing fixed so the new fog of war is starting to behave itself. There is still a bunch to do so my 7 day estimate at the beginning of last week is looking dumber by the day, however it's still a good day.

I can load boards again, walk my character around and see that the systems are communicating properly. The next things on the todo are (in no particular order):

* Make tile deletion work with the fog of war
* Improve performance of adding tiles (see below)
* Only add occluders (a special kind of tile) to the fog of war zones
* Only update tile visibility info for zones that are on screen or nearby
* serialize the fog of war (maybe)


Right now, when loading boards there is an expected but noticeable lag. I know it's related to the fog of war system but I need to know which parts are causing the issue. I'm definitely doing that before serializing the fog of war data as that would just mask any issues.

Right, gotta go get ready for a stream

Seeya
