---
layout: post
title: TaleSpire Dev Log 276
description:
date: 2021-05-22 19:20:49
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks!

After the Norwegian national holidays, I poked around fixing the bugs which caused the board corruption that hit some folks a few weeks ago. Part of the fix involved adding an extra piece of runtime data per tile/prop in the board.

This did not affect the size of the saved data, slab size, etc. However, as this did mean an extra 12 bytes of data per tile/prop at runtime, I spent a while looking at reducing that. From now on, we'll refer to the tile/prop as a placeable.

The data in question was the world position of the origin of the placeable. Positions of props are snapped to the nearest hundredth of a unit, so we don't need the full range of float values. This means we could opt for a different representation. Half-float does not have the precision we need when values are greater than around 1000, but we could multiply the value by 100 and cast to and int (essentially storing as fixed-point), the values happily fit within the range of a `short` so that would take the size increase down to 6 bytes.

Also, all placeables exist inside zones, so we could store the position relative to the zone. A zone is a cube 16 units across, so that means we only need `16 * 100 = 1600` values per component or 11 bits per component. That's ~5 bytes for the three components.

I also looked into other runtime values stored placeable, which could be stored differently now that we have the origin position per placeable. However, I won't overload this update by going into all that.

After all the poking, I could make things smaller, but it didn't result in more placeable data per cache-line, so I didn't think the extra costs would be worth it. I'll definitely re-examine this when I switch to more of an SOA format for some of the data.

Other than this, Ree and I took some time to plan some changes to the persisted per-creature data. We are adding the data for a bunch of features in one go, even though it'll take a while to implement the rest of each feature. This is because upgrading the file format takes a lot of care in order to avoid screwing things up.

We will be adding the data for:

- Persistent emotes
- Four more stats per-creature n
- The ability to add props to bases and use them as creatures
- Polymorph
- Extra color data for things like chat bubbles

You'll be hearing a lot more about those as work continues :)

Have a good one folks!
