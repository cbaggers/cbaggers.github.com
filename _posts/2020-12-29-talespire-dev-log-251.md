---
layout: post
title: TaleSpire Dev Log 251
description:
date: 2020-12-29 01:17:34
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya peoples! Today was my first day back working after the Christmas break, and my goal was to add prop support for copy/paste.

Previously I was lazy when implementing copy/paste and stored the bounding box for every tile. This data was readily available in the board's data, so copying it out was trivial, as was writing it back on paste. However, this is no good when artists need to change an existing asset (for example, to fix a mistake). Fixing this means looking up the dimensions and rotating them on paste, but that is perfectly reasonable.

Looking at this issue reminded me that we have the exact same problem with boards. As we are looking at this, we should probably fix this for board serialization too. This does make saving aboard much more CPU intensive, however. The beauty of the old approach was that we could just blit large chunks of data into the data to save; now, we have to transform some data on save. We will almost certainly need to jobify the serialize code soon[1].

The good news in both the board and slab formats, we will be removing 12 bytes of data per tile[0]. In fact, as we have to transform data when serializing the board, why not make the positions zone-local too. That means we can change the position from a float3 to a short3 and save an additional 6 bytes per tile[2]

A chunk of today was spent umm'ing and ah'ing over the above details and different options. I then got stuck into updating the board serialize code. Tomorrow will be a late start as I have an engineering installing internet at my new apartment at the beginning of the day. After that, I hope to get cracking on the rest of this.

Back soon with more updates.

Ciao

[0] `sizeof(float3) => 12`

[1] Or perhaps burst compile it and run it from the main thread.

[2] technically we only really need ceil(log((zoneSize * positionResolution), 2)) => ceil(log(16 * 100, 2)) => 11 bits for each position component, which would mean 33 bits instead of 48 for the position. However short3 is easier to work with so will be fine for now.

