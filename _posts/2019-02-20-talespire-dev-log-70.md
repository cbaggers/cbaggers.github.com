---
layout: post
title: TaleSpire Dev Log 70
description:
date: 2019-02-20 00:43:51
category:
tags: ['BouncyRock', 'TaleSpire']
---

Evening folks,

I've been messing around with fog of war and whilst it's not there yet there has been progress.

The task right now if fixing [this issue](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/50). The short version of the problem is that we have a data-structure that describes where has be uncovered (we call this a creature's 'memory') and we have to sample it to see if a tile is visible. We can't just sample in the center as maybe a tile is multiple units wide and only the corner has been revealed. We also can't just sample at the center of each sub-tile as we get situations like this


```
    ||
    ||
____|| _____
__A__||__B__|
```

Where `A` is a wall and floor tile and `B` is just a floor tile. If `B` has been revealed then we have to see the wall (otherwise users get confused as to if it's the end of the board or if it's a hidden tile). This means sampling slightly outside of the bounding box of the tile. However this still can cause issues, such as the one linked above.

I played around with a few more schemes but today swapped it out with a mechanism that does a breadth first search of all the nodes within a bounds in the creature's 'memory'. This has a more expensive worst case than the other approaches, but the tests were seeming to show that to take enough samples for a good result we would be paying a pretty high cost anyway. The creature memory is stored in a quadtree so in many cases the search can succeed early as we don't have to search the full depth to find a populated cell.

I haven't got anything more today so I'll bail now and give ya more news soon.

Peace
