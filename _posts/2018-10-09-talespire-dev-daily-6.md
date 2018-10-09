---
layout: post
title: TaleSpire Dev Daily 6
description:
date: 2018-10-09 18:55:50
category:
tags: ['BouncyRock', 'TaleSpire']
---

It was a slow day today. I carried on working on the fog-of-war system which is what works out where in a board a given character could get to. It's being rewritten as the version we have demoed so far did not handle multiple floors.

The behavior needed is that, when a character is placed, everywhere that is accessible in the board should be visible. To do this we are splitting up the (occupied parts of the) board into zones and in each zone we have a structure that describes where is solid. This makes it cheaper to search the zone when ascertaining what you can see, as we don't need to hammer Unity's collision system all the time.

This fiddly thing is we want it to be fast and so I'm trying to balance cache locality of data with wanting a pretty sparse data-structure. I had wanted to make use of Unity's new job system and there you need decent size chunks of 'flat' data. For that reason I had wanted to avoid octrees and look at simpler bucketing instead. My current approach is pretty crappy but I really need to get *something* working so I can start measuring.

The good part is that I added the Zones class and got the low res collision info from yesterday written into the Zone. Tomorrow I will try and not think about how bad performance will be and just write the search.. or if I cant then I will take the search back to the whiteboard :)

Until then, seeya
