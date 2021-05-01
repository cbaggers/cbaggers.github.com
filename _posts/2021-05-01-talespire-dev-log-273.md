---
layout: post
title: TaleSpire Dev Log 273
description:
date: 2021-05-01 19:38:27
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks.

I spent the end of the week looking into the bug with markers which meant they didn't spawn when joining the board.

This went well and allowed me to clean up a bit of the implementation behind the scenes. However, this also meant I bumped into a bunch of other bugs related to GM blocks. I've got through a few of those now, with only one remaining that is stopping me from shipping[0].

The patch fixing markers will come soon. One thing that will be missing from the initial patch is that published boards won't include markers. This will be fixed later shortly (probably later in the week).

I would also like to start hashing the board state that lives in the database (markers, unique creatures, etc.) and skipping the download if the local cache has the latest data. It's not critical, but every little helps.

In the last update, I mentioned that I was sent a published-board and some instructions to replicate a nasty board-breaking bug. Once I had this, I was able to repeatedly delete parts of the board and then test if the bug still occurred. I was able to get the large board down to a small chunk of tiles that still triggered the issue. I then started seeing something odd. Occasionally the delete would corrupt the tiles, and occasionally it wouldn't. I slowed down and took more note of how I was following the steps, and I realized that if the selection box only just enclosed the tiles, the delete didn't corrupt anything. However, if I used a huge selection box that encompassed the slab, then delete would corrupt the slab. This told me exactly where the problem was.

The board in TaleSpire is divided into 16x16x16unit zones. When we apply changes to the board, we often do so in parallel across the zones. A delete typically needs to scan through all the tiles in the zone to see which ones intersect the selection bounds. However, if the entire zone is enclosed by the selection bounds, we know that every tile must be too. This allows us to implement delete more efficiently in those cases. The bug was in this optimized version of delete. It wasn't an exciting bug, just a simple case where I wasn't incrementing an index at the right time[1], but plenty enough to cause havoc. 

I then had to update the tests. First, I added a test for this specific case. But then I updated the fuzzer as that should have been capable of finding this on its own. The problem was simply that the bounds for the deletes being generated were not large enough in all dimensions to enclose zones[2]. With these fixes, we are now covered if some regression were to recreate this problem again in the future.

That's all for now, folks. I'm excited to get the marker fixes out as I'd really like to get working on the marker panel soon.

Peace.

[0] it's a bug which means that if you modify a sector of a board, then the gm-blocks in that sector aren't synced to other gms when they join the board.

[1] There was a similar mistake in the undo code for this kind of delete.

[2] The model we fuzz against is a 1d board, this meant that we only ever needed thin selection bounds. It doesn't hurt to use larger ones so we do that now.
