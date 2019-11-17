---
layout: post
title: TaleSpire Dev Log 128
description:
date: 2019-11-16 16:56:16
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Update time! Progress has been very good.

@Ree has kept on hammering away at the building tools from the last update. He's also been handling lots of behind-the-scenes organizational stuff which whilst not exciting to blog about is as critical to TaleSpire happening as anything else we do.

Jason has been chatting to loads of the backers who pledged at the 'Help design a ___' levels and sculpting is in full swing. Very exciting!


I've finally broken through the wall of fiddly details that was plaguing my board sync implementation and it's looking pretty good now. This has freed me up to look at some other tasks that are on my todo list so I'm gonna ramble about that for a bit in this post.

First off I went back to Fog of War. Basically, the task is this:

> 1. Take a 16x16x16 grid
> 2. In every cube write 'true' if there is meant to be fog there and 'false' if not
> 3. Now take this info and make a 3D mesh that contains all the cells marked 'true'

And I wanted to make sure we had step `3` worked out.

Now there are a couple of points to keep in mind:

- The mesh we need to make needs to conform fairly closely to the grid as we don't want to see glimpses of anything we shouldn't
- We don't need to make this look like fog, we just need a mesh we can start working with, we can do all kinds of polygonal and shader effects on top of a simple mesh that will look much more fog-like.

As a great example check out this tweet from [@Ed_dV](https://twitter.com/Ed_dV) !

https://twitter.com/Ed_dV/status/1169153878841004033

Those clouds are spheres, with lots of magic on top (if you watch the whole thing it shows the spheres without the magic).

Given those two points, I decided a good source of wisdom would be the kinds of algorithms that Minecraft-like games often use. Luckily for me [Mikola Lysenko of 0fps.net](https://0fps.net) has written [some amazing articles on these techniques](https://0fps.net/category/programming/voxels/page/1/) along with WebGL demos and source code, what a star! This gave me a huge boost and along with a few implementation tips from other sources I was able to put together a 'Greedy Mesher' (the name of one of the techniques) in Unity. Now, this next picture is *not* meant to be clouds, it was just a test to make sure it was working.

![/assets/images/jobifiedMesher0.png]

For the coders out there, I also made this implementation using Unity's job system which allows me to run the mesh generation jobs in parallel across all cores. It's pretty speedy considering how little effort was required.

That is all on the subject of fog for now but we will be back with more in future.

-------

Next on my list of concerns was interactable Tiles and modding.

We have a bunch of interactable tiles in TaleSpire and this is only going to increase as we open up the ability for the community to make them.

![/assets/videos/sideTable0.gif]

A real pain point when you make a  user-driven content game like TaleSpire is that you have no way to know when or where someone will just suddenly throw tonnes of extra stuff for your game to do.

For example, take that side-table in the gif above. There is nothing to stop you dragging out a whole load of tables and the numbers get large FAST. Remember that if you drag out a square with 32 tiles down one side then that is over 2000 tiles that your game suddenly has to handle. They all need to appear on all the other player's machines quickly and they all need to be interactable immediately.

That means all 2000 of them are running their own little scripts. How do you ensure that they don't start crippling your game?

Now we naturally knew this was coming and previously we had been making small fast components that would then be wired up using a LUA script (that's another programming language for those not into this stuff :] ).

It worked but I was still concerned about how it would scale.

There was another thing. Like I said we were using LUA in a weird way, as a way of setting stuff up. I started thinking that that might be the worst of all worlds for users as, for those who like LUA, there are lots of things they would be told not to do and for those who hate LUA, well they hate it :p

Were there other problems? You're damn right there were!

As mentioned before, we need to keep all of these tiles in sync on everyone's machines and so the idea of scripts just being able to fire off messages whenever they liked was a nightmare, it could really make the game unstable if done wrong.

Now, this sounds tautological but there are two kinds of state for a tile: State that matters to the story and state that doesn't. For example, whether a fire is lit or not matters, the exact positions of every smoke particle does not. As long as it roughly conforms all is well.

So it would be good if we can separate these kinds of scripts out from each other so we have a shot at being able to enforce sensible behavior over the network.

So after pages and pages of notes and a lot of coffee, I decided it would almost be worth making our own compiler that suited our needs but we needed

Now, this bit gets technical so feel free to skip to the row of asterisks!

+++++++++++++++++++++++++++++++++++++++++++++++++++

I wanted something that

- has a simple execution model.
- was focused on only the tasks we need.
- run on multiple cores
- used no GC
- was reasonably fast

Here's the approach I went for:

0.
I decided to start with only 4 data types: int, float, int3, and float3

1.
I use a node graph as the code representation. I'm currently using the excellent [XNode](https://github.com/Siccity/xNode) but if/when Unity's new graph UI stabilizes I will probably move to that.

I added code to detect cycles and compute the correct types at each node

2.
Walk the graph and generate a tree of [IR](https://en.wikipedia.org/wiki/Intermediate_representation) nodes. This is mainly for convenience but in the conversion, we also pick correctly typed IR nodes in a few specific places.

3.
Walk the IR nodes to compute the layout for the stack

4.
Walk it again and generate a bytecode that represents the program

5.
Do a little cleanup pass and kick the result out as a `NativeArray<byte>`

6.
Write a [Job](https://docs.unity3d.com/2018.3/Documentation/Manual/JobSystem.html) that takes the instruction array, an array of private state and a few other bits and run our little bytecode in parallel.

7.
Marvel at the power of coffee


***************************************************

It took 18 hours to get the first version up and running and the results were promising.

Here is a picture of some nonsense 'code'

![/assets/images/spaghet0.png]

The graph above (whilst being gibberish) can be run for 10000 tiles in around 2ms, which is way slower than it will be but it's acceptable for a first pass. It told me the approach was worth more work so I took Friday to flesh it out a bit.

Oh, and the language is called Spaghet!

With that we have the kind of script that can be used for the 'not story critical stuff' but we still need the other scripts too.

To that end, I'm now working on how to script the [state machines](https://en.wikipedia.org/wiki/Finite-state_machine) that will run the important stuff. Here is a little picture of the prototype I'm currently working on:

![/assets/images/spaghet2.png]

Naturally, the look of the graph and the nodes available will be improved too.

Soooo yup, it's been a good, busy week. I'm going to be working on this and all the code that holds it together all this week. I'm hoping to have the major plumbing done by Wednesday though.
