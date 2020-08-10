---
layout: post
title: TaleSpire Dev Log 211
description:
date: 2020-08-10 15:05:16
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Finally, something decent to show again!

I won't lie, the last couple of weeks of coding have been hard. I've been working on the new animation system, and, as you'll see, this dragged in a LOT of other systems. While the final solutions are not overly complicated, finding out what they needed took a lot of tries, and the number of systems involved made it hard to keep in my head.

First, though, videos time!

The following clip shows 10k animated objects being spawned, all with their own scripts running independently, without dropping below 60fps. They are all in sync as I wanted to see lots of things happening, so I made time loop every 2 seconds, causing the animations to replay.

<video controls src="/assets/videos/clams.mp4" type="video/mp4" width="620"></video>

This might be a bit underwhelming to look at, but I'm so glad to see this. Let's dive into why.

### Scripting

For a long time, we knew we needed to be able to add behaviors to tiles and props. Without hackery, Unity doesn't let you add more code at runtime, so we can't just load c# code from asset-packs along with tiles and creatures. This got us looking into scripting languages.

We started looking at LUA. The C VMs are fast, but there are also c# VMs that have excellent integration with other c# datatypes.

> Side note:
>
> A lot of the time, my job is looking at something simple and saying the words, "And then the user spawns 10000 of these" (let's call this the '10k-case'). Making 10k of something in TS is very easy, just drag out a 100x100 region. This is why everything in user-generated-content games is more complicated than it seems.
>
> My role is to look at this scaling problem try and limit the worst-case complexity. The players will always be able to make enough things that the game performs poorly, but we usually can find things to improve the situation.

Looking at various scripting languages, my big concern is what happens when 10k are spawned as:

- Each needs an unknown and potentially unbounded amount of memory.
- Each might be used in ways that aren't thread-safe.
- Everything object created needs managing by the GC.

The last one is especially concerning. If you've ever shipped a game in a managed language (like c#), you've probably spent time minimizing memory allocations to stop the GC from impacting your frame rate. Without understanding the impact of a script, I was unsure we could make something that we knew would run well.

The next experiment was to use LUA like a scriptable composition system. We would provide performant 'operators' and modders would connect them up using a LUA script, which would be invoked less frequently (for state machines it would be on state change).

This was promising, but we are in this weird case where we have made LUA not like LUA anymore. So the people who like LUA and want to use it cant. And the people who hate LAU still have to use something they won't like. And we still have the threading questions.

### Spaghet

This made me take a day to hack together a simple compiler. It produced a very simple byte-code that we would run on a little VM built on top of Unity's Job System. By using the job system, the VM code is optimized by LLVM and we can run the scripts on many threads in a way that fits in well with our other systems.

Even simple tests showed promise. We split the world into two kinds of scripts:

- State-machine scripts
- Realtime script

State-machine scripts (currently) run only in response to user input, and they are fully synchronized across the network.

Realtime scripts can run every frame and are not synchronized (as there would be far too many messages). Their programming model is similar to shaders in that the tile's visual state is recreated from scratch every frame from the input.

The trick now is that each state of a state-machine script can have an associated realtime script. This means that when in that state, the realtime script is run automatically.

For example, for a door, the state only changes (and is synchronized) when the user opens or closes it. However, by animating the door in the realtime scripts, you get the expected animation on all player's machines at the right times.

### The fall of the GameObject

Unity's traditional approach to making this is all object-oriented. It's compelling and, in the right hands, is an incredible canvas for creativity. Alas, when performance is critical object-oriented programming (OOP) is problematic. OOP makes it very hard to control where things stored are in memory, and this is essential when trying to batch jobs together to run quickly. CPUs can do unfathomable amounts of work per frame if they aren't spending their whole time waiting on things to arrive from RAM.

![relative speedasnimation](/assets/videos/cacheanim.gif)
> Memory access latency from cpu caches v ram – Andreas Fredriksson

On top of this, Unity's GameObject's are (relatively) slow to create, fine for most games, but terrible for our 10k-case.

We worked very hard trying to keep using GameObjects as they have such significant advantages for content creation and experimentation.
Arguably, this contributed to the delay of the Beta as working around the speed limitations had some very non-obvious interactions with our building systems. However, it was not to be, and even though the Beta shipped, we continuously see performance issues that we don't have clear solutions too if we stick with the current approach.

Of course, we've known about this issue a long time, and the good news was that an answer was already in production. Unity's 'Entities' system is their new approach to handling things in the game and promised impressive performance boosts. We were very much banking on that solving our issues.

This was a mistake on my part. Naturally, the Entities system is a complicated beast with its own tradeoffs, and Unity is taking its time to get it right. When we finally tested the Beta, it still had some concerning performance characteristics for our use case. That sucked.

The good news was that amongst all this new code, there were many high-performance tools that we could use to make our own system.
The problem is that, with GameObjects, we got so many things for free and now we need to handle them ourselves. Here are a few concerns:

- The new physics engine is excellent, but we use collision casts *everywhere* so most systems in the game are gonna need some modification
- There is no culling or batching written for us. We need those.
- There is no animation system at all
- There is no way to specify lights expect using the classic approach
- feel: This is a terrifying one. Game feel takes ages of tiny tweaks based on the creators' intuition. If we lose that it'll take a long time to recreate

We knew we had to just go for it. As tiles and props are where the perf issues lie, if we can make a new system for them, we can afford to keep the other stuff as GameObjects.

### Too late now >:)

Anyway, there was nothing to do but to get started. I jumped over to TaleWeaver (our modding tool) and started work on the new data formats for everything we would need.

It was critical that we didn't mess without our artist's ability to work. Making games is hard, and everyone needs to be able to work as effectively as possible. Your smart little ideas and solutions must not tread on someone else's ability to work. To me, this meant analyzing the GameObject representation in TaleWeaver, and writing out our own, TaleSpire specific, version of this data.

Luckily once again, new tools exist in Unity to help. The Entities package has a Blob data system that promised fast and job-safe data encoding un unmanaged memory. I made some tools that let it hook into Unity's classic asset serialization system and got to work.

I extract animations, compile scripts, flatten transform hierarchies, and other simple stuff and pack the result into out blobs. Most of the slowdown here is split between learning new things in Unity, and working out what we need. We also made atlases from the tile icons.

I replaced Spaghet's graph UI and iterated on the VM. Again, most of the time is just from working with new tools and trying to find the best ways to do the silly things we need.

On the TaleSpire side, I wrote a new asset database to accommodate the new formats we have made and hooked up enough of that to keep working[0]. I then started work on the batcher.

The batcher is a beast. Each frame we need to tell Unity what to draw and Unity, like any engine, needs that information in some format it can use. Luckily for us, most of our objects don't move each frame, so if we can get it in the correct format when the board changes, we can keep reusing that data. From now on, we will use the term 'statically-batchable' to talk about all the things where we can use this technique. All of the objects where we do need to update the position/rotation/etc per frame we will dub 'dynamically-batched'.

So I spent a day or so writing the static batcher. Each zone (16x16x16 unit region of space) in the game runs this independently and concurrently. We do not combine batches across zones. This means we make more draw-calls than we *could*, but this lets us enable and disable zones without recomputing the batches for any other zone.

Dynamically-batched content is harder. I had already decided that the animation system would be directly tied to the scripting system, that is, Spaghet will be animating the objects. That lets us simply say that any part of the tile that can be modified by a script during runtime is dynamically-batched, and the rest are static[1].

I spent some days getting the Spaghet wired up enough to continue. I then made jobs to run the realtime scripts, compute the transforms for the objects and write them into the batches. A fun thing to note is that the number of dynamically batched things is not changing frame to frame. This means that you can do the following:

- Size a batch with enough room for every dynamic & static instance you need
- Write the static batch info leaving enough room at the end for the dynamic ones
- Every frame write the updated dynamically-batched transform into the reserved space

This is nice as you don't need any extra allocations or draw calls. The downside is the complexity it adds to the system, but hey, we are here for the performance.

During all this, I kept finding small issues that meant I needed to change the data format to help the engine. This is the right thing to do, but switching back and forth between the projects was tough on the brain.

I'm gonna skip a pile of details for all of our sakes, but the result is that the thing we are now making is capable of getting us where we need to be. It's not fast enough, or stable, but the tests are finally showing something promising.

> * Wrong thing to conclude: Unity is slow
> * Right thing to conclude: Unity **is** fast. But, for what we need, the default tools they have to drive are not.

Warning about performance numbers. We can't make decent measurements of the game yet, I still need to write up a lot of new stuff before we can start getting metrics we can rely on. But what we can guarantee is that TaleSpire is getting faster.

I hope you enjoyed this. I also hope I managed to convey the reason I've been less mentally available recently. Things are going to stay ugly for me for most of this month, but I know we will have something cool in the end.

Have a great day.

Peace.


[0] Funnily enough, the old one is still in there now being used by the UI. I'll drop it when I get to that part of the rewrite.

[1] there are obvious cases where we don't have to recompute a tile's transform every frame, for example, when an animation has finished playing. This is true and will be handled by Spaghet scripts knowing when they can go to 'sleep'. I'll cover this in another post.
