---
layout: post
title: TaleSpire Dev Log 356
description:
date: 2022-10-10 00:49:43
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks.

I'm back again with a group-selection update. In short, progress is very good.


I spent a couple of days finding a control scheme that felt good. I wanted the interaction to be based around a single modifier. You hold down a key (in my test, `x`), then you left-click and drag to draw the lasso. You can also single-click on a creature to add/remove them to/from the selection. It sounds simple, but I managed to go in circles a few times, ensuring that switching between single creatures and groups felt natural. It's easy to add extra modifiers and make things feel like a tool. I'm happy to have somewhat avoided that.

Having spoken to Ree, we've also discussed trying another approach to the lasso control. I'll talk about that more as it develops.

With the behavior taking shape, I turned to performance. I knew from the start that the regular C# implementation would be too slow. I wrote the code assuming that I'd be switching to compiling with Burst and using native collections. My initial tests[0] showed that updating and meshing a sizable wobbly lasso took between 14 and 17 milliseconds, which is hilariously bad[1]. By compiling with Burst, that was chopped down to a few milliseconds.

From there, I bucketed the lasso segments spatially so that intersection checks had to consider fewer segments. With this and a few more techniques, I dropped the time required to around 0.3 milliseconds.

The next part of the optimization was moving the whole process (including setting up the Unity mesh) into jobs. I spent quite a while playing with different configurations[2] until settling on what we have now. The lovely thing is that, in the typical case, the jobs are finished before the main thread needs the result. This results in the main thread only having to spend ~0.01ms on updating the lasso and mesh.

While there is definitely more I can do to optimize the lasso code, it's plenty good enough for now[3]. It was then time to work on other actions that groups of creatures could perform. If you've been on our discord, you'll have seen some clips of that work. If not, then here you go!

![group base color](/assets/videos/groupBaseColor.gif)
![various group actions](/assets/videos/groupMisc.gif)
![persistent emotes](/assets/videos/groupStatus.gif)

I'm not trying to support everything out of the gate, but some things just felt like they needed to be there.

Well, that's all from me for now. While I have started looking into performance improvements needed for the macOS release, I'll leave the details of those for another day.

Have a good one!


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*


[0] I'm talking about timings, but it is very misleading. The time needed to update the lasso depends on many things, such as the lasso length, the number of self-intersections, etc. For narrative reasons, I'm going to mention numbers, but the only thing to really take away is that they got better :)

[1] For those not familiar. To run at 60fps, you need to be done with *everything* for a frame in 16.6ms. Spending that time just for a lasso is naturally unworkable

[2] For example, I experimented with running the meshing of separate lasso chunks in parallel. This was promising, but the overheads undid the benefits I saw from the concurrency.

[3] There are lots of other things to optimize before this one creeps back to the top of the list :)
