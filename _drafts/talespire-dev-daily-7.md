---
layout: post
title: TaleSpire Dev Daily 7
description:
date: 2018-10-10 23:04:50
category:
tags: ['BouncyRock', 'TaleSpire']
---

Here we are again. This time I don't have much to show as I spent most of the day at the whiteboard. So instead I'll talk about the problem I've been mulling over.

When a character piece (henceforth just called a 'character') is placed on a tile we want to show every tile it could possibly reach, regardless of distance. We dont want to show things behind walls or locked doors. Also we need to do this quickly.

Actually let's pause to clarify something. It is going to be tempting to redefine the problem given the issues that we are about to look at. You may not even like the idea of the behavior and want to change it for that reason. Please, for now, trust that we have tested this in game and it feels nice, so for now this is the problem we are trying to solve.

Alright, back to the snooker..

Before the game had floors it was effectively 2d, the approach then was to [flood fill](https://en.wikipedia.org/wiki/Flood_fill), performing raycast to see if there was an occluder (a wall) that would stop the progression. It might be rather wasteful but it worked well enough for a demo and you can get away with a lot of raycasts per frame when your game is as simple (in terms of how much stuff is going on) as ours.

However now we add floors and everything changes, now we have a third dimension to reckon with and everything gets much more expensive. One other interesting detail is although we have separate floors we also want the users to be able to make features that reach through to other floors. For example imagine a grand hall with a obsidian pyramid in the center, we want to show the whole pyramid from the ground floor even though it is several floors high as this gives the moment gravitas that would be lost if it you only saw a tenth of it.

One basic thing we did in the 2d version and will still do in the 3d version is divide the world up into zones. A zone is a 3d region of a certain number of tiles in the X and Z dimensions and some height (lets say 10 floors worth) from a given floor.

With this known we specify the subproblem as: Find every reachable place in the zone and which zones we can access from this zone.

Also it's worth noting that the player will be moving the character frequently so the result should be cachable or very cheap to recompute.

In my mind we need to know what is solid and what is empty space. Let's say the zone was 81x81 tiles in size and just as high as it is wide. The tiles are 1 Unity unit in size so that's 81x81x81 units, assuming we only need to know if a unit cube is solid or not then a dumb solution would require us to store 531441 booleans (true or false) so say whether each part of the zone is solid or empty. Clearly for large boards this is untenable.

One common and reliable way to store 3d spatial data more efficiently is using an [octree](https://en.wikipedia.org/wiki/Octree). It recursively subdivides space into 8 equal subspaces down to the required resolution. So (for a 2d example), rather than having the 2d array on the left with 36 slots we have the approach on the right with 10 slots as we don't subdivide if there is no new information inside.

![quadtree](/assets/images/d7_0.jpg)

> yup it's not a totally accurate drawing but I hope ya get the idea

Each node in the quadtree has, either a marker saying it's solid/empty or has 4 child nodes.

![quadtrees are trees](/assets/images/d7_1.jpg)

For octrees it's the same deal but with 8 nodes and 3 dimensions.

So, now we have our data-structure we can fill it with information about what is solid and what isn't. Then we could so something rather similar to the old flood fill, only this time we start from the character position, find what region in the octree they are in and walk to the neighboring regions.

Most of the time we move somewhere it's going to be one of the places we have already searched so we can cache the result of the previous walk. We can also cache the octree itself.

So far so good, but there are issues:

![woops](/assets/images/d7_2.jpg)

a. Our 3d search can search over walls we need to limit that but that will limit their
b. Not having to search to work out what parts of the quadtree/octree are accessible would be cool. They have that built in recursive structure so can we leverage that?
c. Tiles are place on the floor itself or on other tiles, can we propagate information between them on creation that could help us? could we serialize it when saving the level so we dont have to recompute on load?

`b` gets rather tantalizing. If we could asign each node in the tree an integer id, we could then take the `min` of the neighbours and it would resolve to all nodes having the same id if they are reachable. The resolve takes time though, could we find a way to do this in 1 (or some fixed low number) of passes? Is it worth it?

![auto-tag](/assets/images/d7_3.jpg)

Unrelated but important; if you want to be fast then [cache locality](https://en.wikipedia.org/wiki/Locality_of_reference) matters. Could we store the data in a way that helps our intended access patterns (searching)? One example could be using a [Z-order curve]() to linearize the data and using something like a skiplist to keep the data sparse.. again would this actually help? It all depends on our data and how we access it.

I've been asking a lot of questions as it frames the issue nicely, but I do have some WIP answers but explaining them will balloon this post even more so for now I'll just say I hope to have something I can show by the end of Sunday. I'd also like to stress that these problems arent novel, there is a lot of literature and most games will be dealing with much more interesting cases than this.. still it's fun to think about.

Until tomorrow,
Ciao
