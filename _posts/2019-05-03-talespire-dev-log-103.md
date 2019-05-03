---
layout: post
title: TaleSpire Dev Log 103
description:
date: 2019-05-03 20:08:36
category:
tags: ['BouncyRock', 'TaleSpire']
---

Heya folks,

Programming has over the years reminded me again and again that problems with trivial descriptions often hide catacombs of complexity, and that any time you use the word **just** to yourself you are probably in trouble.

'I **just** need to do this `<insert simple thing here>`' is a ancient spell than summons bugs and leads you into sleepless nights of crunch. SO.. when yesterday I was commenting on how copy paste would be difficult I thought I was speaking sensibly and from experience, and thus the gods of code decided that I would get smacked in the face with my own ignorance yet again.

This a long winded way of saying. This works

![I know nothing](/assets/videos/copyWip2.gif)

Which is nice, if mildly bewildering.

There are of course issues and general lack of polish. One side effect of this feature is that it really makes it easy to run into other bugs in the alpha. It's now very easy to make very large, very tile heavy boards. Very easy to duplicate areas which already have issues with fog of war and amongst other things our selection tools still aren't great with large piles of assets.

We'll may have this in your hands next week so be gentle :D

With this working it made it possible to save out simple data that described prefabs. This will make it possible to save small sets of tiles to use as building blocks in game. In future we will have ways of sharing these but our first order of business to bring something similar to our current tiles. For example consider this tile:

![tavern wall](exampleTile0.png)

It's wonderful to have tiles like this but maybe you want to remove the bellows and add a lamp there, these changes could let that happen (in time). Again there are lots of details to get this to work well and it will mean updating a lot of existing assets, but we are going in the right direction.

Until next time.

Peace!
