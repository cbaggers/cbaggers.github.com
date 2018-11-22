---
layout: post
title: TaleSpire Dev Daily 39
description:
date: 2018-11-22 23:26:36
category:
tags: ['BouncyRock', 'TaleSpire']
---

Happily it's all still going well. I started the day switching the board syncing from http to https, a small task but of course it's very important. The only reason it hadn't been done already what that things were being built quickly to test the mechanism. I'll ramble on about my future plans regarding security another day.

Thanks to some recent changes to the board-asset code we now know the place the pieces will land the moment they are released by the player. This lets us start the fog of war update a few frames earlier which makes things feel snappier.

Also thanks to [@jonnyree](https://twitter.com/jonnyree) adding drag-select I added deletion of multiple tiles. It feels so good to crazily mash ctrl-z and ctrl-y on one client and see that the result is totally stable on the other. I know of a few places where things could go wrong though so I am fixed them up tomorrow.

I'll post a video of the undo/redo tomorrow too, even though it's doing what it has to do I'm still stoked to see it doing it so well :)

Alright, time to hit the hay.

Ciao
