---
layout: post
title: Hammering out the api
description:
date: 2016-01-11 18:58:19
category:
tags: ['lisp', 'cepl']
---

### Where we left off

As I said the other day I have #'get-transform working, albeit inefficiently, and after that it was time to think about how these features should be used. Of course we have `in` blocks, but there were questions (that I mentioned last time) like:

- What happens if you try to pass a type like position or space (that don't exist at runtime) from one shader stage to another?
- Can these kind of types be used in gpu-structs?
- Should stages have an implicit space?
- If you pass up a position as a uniform, where does it's space come from?

Questions like this have been taking a lot of time to sort out. It's one of those funny things that, when the language doesn't limit you and your goal is 'the best' programming experience, then any of hundreds of crazy solutions are possible so choosing becomes harder.

### Maybe this

One possibility was to make space a first class concept in cepl, so all gpu-arrays and such would have a space. That would mean that I could have the terrain vertex data in a gpu-array in world space. e.g.

     (make-gpu-array terrain-data :type 'terrain-vert :space world-space)

and then the vertex shader could technically look like this

	 (defun-g terrain-vertex-shader ((vert terrain-vert))
	   (values (pos vert)
			   (normal vert)
			   (uv vert)))

Now! because the vertex shader implicitly takes place in clip space and cepl knows that the vert data is in world-space then it could add the world->clip space transforms for you.

This sounds kinda awesome, and with cepl I can totally make this work... but I decided against it. Why? Well because it bakes a certain use-case into a data-structure, here is a simple example where it breaks down.

I have the data for a bush, I will instance this a thousand times across a landscape, each tree will have a different model->world transform. At this point what is the space associated with the bush data doing? It's not needed yet it's 'attached' to the data.

An hour of wrangling around that idea made it seem like this idea just added complexity so, for now at least, it's out.

A few more ideas ending up in a similar place. Namely:

> cepl, in some places, is reaching the limit of how much it can help without becoming an engine.

this is an interesting feeling in itself.

So after all that I decided the healthiest thing was to leave that for a bit and focus on the cpu side of the space feature.

### Goblins of the brain

One thing that was odd was that I had this nagging feeling that I had *something* wrong with my concept of how spaces should be organized so I've been staring at books & tutorials again trying to find the something I had missed. Quite suddenly the following popped into my head:

> I have assumed that all spaces have one parent. I have conflated hierarchical and non-hierarchical relationships

And now I need to try and explain what I mean by that :)

The whole point of making a space graph rather than a scene graph was to avoid the idea that game entities would be part of the graph, which I believe is a massive code-smell.

I want only spaces to be in the graph, so far so good.

I define my spaces with some transforms relative to a parent space. Seems OK. But is it? I realized I ran into some issues when looking at the eye-space to clip-space relationship.

There isn't a clear parent. Changing the position of the eye (game camera) doesn't transform clip-space in the way moving a table affects the objects on the table. Also defining eye-space as a child of clip space feels weird.

What I had was what I'm calling a non-hierarchical relationship. There **is** a valid transform between them but it isn't defined in terms of a parent-child relationship. Examples of hierarchical things like arms and hands, where moving the arm moves the hand.

So what I want now is:
- lots of space-trees, which are hierarchical. They are defined in a parent-child way and are great for character limbs, foliage branches etc.
- non-hierarchical relationships between the root nodes of those space-trees.


So is this special? Nope :) It more my journey than something totally new. The first part of the definition from Wikipedia says

> A tree node (in the overall tree structure of the scene graph) may have many children but often only a single parent

but later clarifies:

> It also happens that in some scene graphs, a node can have a relation to any node including itself, or at least an extension that refers to another node.

OpenSceneGraph has multiple parents...which to me sounds like a bit of a terminology mistake, what does a spatial hierarchy with many parents mean?

Also having graph pointers sounds scary, they would have to be quite strict in what they can do..and if they have strict behavior then surely there can be a better term.

Some other engines avoid the issue somewhat by keeping the scene graph to the hierarchical stuff and let the subject of clip-space and such be a concern of shaders. My brief look a Unity seems to suggest this approach.

Whilst that last one avoids confusion at first it feels like you kind of only defer the issue to another part of the code-base. I would hope we could come up with some analogy in code that extends across these domains. This may not be as easy at first but should be simpler in the end.

### And Now

Since that realization I've been sketching out my plan for how to make this. It isnt a massive overhaul which is nice and there seem to be some obvious places to start optimizing later on.

For now I'm just going to get something slow working and play with it to see what happens, but I have to say I feel like a mental blockage has been free'd and I'm pretty optimistic about what comes next.
