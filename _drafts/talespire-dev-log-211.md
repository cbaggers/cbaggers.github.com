---
layout: post
title: TaleSpire Dev Log 211
description:
date: 2020-08-10 15:05:16
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Finally, something decent to show again. 

I won't lie, the last couple of weeks of coding have been hard. I've been working on the new animation system and, as you'll see this dragged in a LOT of other systems. Whilst what the final solutions are not overly complicated, finding out what they needed to be took a lot of tries and the number of systems involved made it hard to keep in my head.

First though, videos time!

This clip shows 10k animation objects being spawned, all with their own scripts running independently, without dropping below 60fps. They are all in sync as I havent yet added physics to these objects and so I can't open the menu to say close :P. So I just made time loop every 2 seconds which causes the animations to replay.

![boom]()

A bit underwhealming to look at but I'm so glad to see this. Let's dive into why.

### Scripting

So for a long time we knew we needed to be able to add beahviours to things. Without hackery Unity doesnt let you add more code at runtime, so we can't just load c# code from asset-packs along with tile and creatures. This got us looking into scripting languages. 

We started looking at LUA. The C vms are fast, but there also exist c# vms which have very good integration with other c# systems. 

> Side note:
>
> A lot of the time, my job is looking at something simple and saying the words "And then the user spawns 10000 of these" (let's call this the '10k-case'). Making 10k of something in TS is very easy, just drag out a 100x100 region. This is why everything is more complicated than it seems in user-generated-content games. 
> 
> I see my role in this to look at that scale at try and limit the worst case complexity. The players will always be able to make  > enough things to make the game perform badly, but what can we do to help the situation.

Looking at the scripting languages my big concern is what happens when 10k are spawned as:

- Each needs an unknown and potentially unbounded amount of memory. 
- Each might be used in ways that aren't thread safe. 
- Everything object they make needs managing by the GC. 

The last one is especially concerning. If you've ever shipped a game in a managed language (like c#) then you've probably spent time minimizing memory allocations in order to stop the GC from impacting your frame rate. Without being able to understand the impact of a script I was unsure we could make something that we knew would run well.

The next experiment was to use LUA like a scriptable composition system. We would provide performant 'operators' an you would connect them up using a LUA script which would be invoked less frequently (for state machines it would be on state change).

This was promising but we are in this weird case where we have made LUA not like LUA anymore. So the people who like LUA and want to use it, cant. And the people who hate LAU still have to use something they won't like. And we still have the threading questions.

### Spaghet

This made me take a day to hack together a simple compiler. It produced a very simple byte-code that we would run on a little VM that was built on top of Unity's Job System. By using the job system the VM code is optimized by llvm and we can run the scripts on many threads in a way that fits in well with our other systems. 

Even simple tests showed promise. We split the world into two kinds of scripts:

- State-machine scripts 
- Realtime script

State-machine scripts (currently) run only in response to user input and they are fully syncronised across the network. 

Realtime scripts can run every frame and are not syncronised (as there would be way to many messages). Their programming model is similar to shaders in that the visual state of the tile is recreated from scratch every frame from the input.

The trick now is that each state of a state-machine script can have an associated real-time script. This means that when in that state, the realtime script is run automatically. 

For example for a door the state only changes (and is syncronized) when the user opens or closes it. However by animating the door in the realtime scripts you get the expected animation on all players machines at the right times.

### The fall of the GameObject

Unity's traditional approach to making this is all object-oriented. It's very powerful and, in the right hands, is an incredible canvas for creativity. Alas when performance is critical object-oriented programming (OOP) is problematic. It's very hard in OOP systems to control where things are in memory and this is essentially when trying to batch jobs together to run quickly, cpus can do unfathomable ammounts of work per frame if they aren't spending their whole time waiting on things to arrive from RAM.

![ram-cache animation]

On top of this Unity's GameObject's are (relativly) slow to create, fine for most games, but terrible for our 10k-case. 

We worked very hard trying to keep using GameObjects as they have such big advantages for content creation and experimentation. 
Arguably, we probably could have shipped the beta sooner this wasn't an issue as it has some very non-obvious interactions with our building systems. However it was not to be, and even though the Beta shipped, we constantly see performance issues that we don't have clear solutions too if we stick with the current approach.

Now of course we've known about this issue and long time but the good news was that an answer was already in production. Unity's 'Entities' system is their new approach to handling GameObjects and promised some impressive performance boosts. We were very much banking on that solving our needs. 

This was a mistake on my part. Naturally the Entities system is a complicated beast and they are taking their time to get it right, when we finally tested it it still had some concerning performance characteristics for our use case. That sucked.

The good news was that in all this new code there were a lot of high-performance tools which we could use to mak our own system. 
The problem is that so many things we got for free in the old approach our now our problem. Here are a few concerns

- The new physics engine is great but we use collision casts *everywhere* so most systems in the game are gonna need some modification
- There is no culling or batching written for us. We need those.
- There is no animation system at all
- There is no way to specify lights expect using the classic approach
- feel: This is a terriffying one. Game feel takes ages of tiny tweaks based on the creators' intuition. If we lose that it'll take a long time to recreate

We knew we had to just go for it. As tile and props are where the perf issue lies, if we can make a new system for them then we could afford the other stuff staying as GameObjects.

### Too late now >:)

Anyway there was nothing to do but to get started. I jumped over to TaleWeaver (our modding tool) and started work on the new data formats for everything we would need. 

It was critical that we didnt mess without our artist's ability to work. Making games is hard and everyone needs to be able to work as effectively as possible. Your smart little ideas and solutions must not tread on someone elses ability to work. To me this meant analysing the GameObject representation in TaleWeaver and writing out our own, TaleSpire specific, verison of this data. 

Luckily once again new tools exist in Unity to help. The Entities package has a Blob data system that promised fast and job-safe data encoding outside of the garbage-collector. I made some tools that let it hook into Unity's classic asset serialization system and got to work.

I extract animations, compile scripts, flatten tranform hierarchies and other simple stuff and pack the result into out blobs. Most of the slowdown here is split between learning new things in Unity, and working out what we need. We also made atlases from the tile icons.

I replace Spaghet's graph UI and iterated on the VM. Again, most of the time is just from working with new tools and trying to find the best ways to do the silly things we need.

On the TaleSpire side I wrote a new asset database to accommodate the new formats we have made and hooked up enough of that to keep working[0]. I then started work on the batcher.

The batcher is a beast. Each frame we need to tell Unity what to draw. Unity (like any engine) needs that information is some format it can use. Luckily for us, most of our objects don't move each frame so if we can get it in the right format when the board changes we can keep that around. From now on we will use the term 'statically-batchable' to talk about all the things where we can use this trick. All of the object where we do need to update the postion/rotation/etc per frame we will dub 'dynamicly-batched'.

So I spent a day or so writing the static batcher. Each zone (16x16x16 unit region of space) in the game runs this independenty and concurrently. We do not combine batches across zones. This means we make more draw calls than we *could* but it means we can enable and disable zones without having to re-compute the batches for any other zone.

Dynamically-batched content is harder. I had already decided that the animation system would be directly tied to the scripting system, that is, Spaghet will be animating the objects. That let's us simply say that any part of tile that can be modified by a script during runtime is dynamically-batched and the rest are static[1].

I spent some days getting the Spaghet wired up enough to continue. I then made jobs to run the realtime scripts in jobs, compute th e transforms for the objects and write them into the batches. A fun thing to note is that the number of dynamically batched things is not changing frame to frame. This means that you can do the following:

- Size a batch with enough room for every dynamic & static instance you need
- Write the static batch info leaving enough room at the end for the dynamic ones
- Every frame write the updated dynamically-batched transform into the reserved space

This is nice as you don't need any extra allocations or draw calls. The downside is the complexity it adds to the system, but hey we are here for the performance.

During all this I kept finding small issues that meant I needed to change the data format to help the engine. This is the right thing to do but the back and forth was tough on the brain.

I'm gonna skip a pile of details for all of our sakes, but the result is that the thing we are now making is capable of getting us where we need to be. It's not fast enough, or stable, but the tests are finally showing something promising.

> * Wrong thing to conclude: Unity is slow
> * Right thing to conclude: Unity is FAST. But, for what we need, the default tools they have to drive are not.

Warning about peformance numbers. We cna make decent measurements of the game yet, I still need to write up a lot of new stuff before we can start getting metrics we can rely on. But what we can guarentee is that TaleSpire is getting faster.

I hope you enjoyed this. I also hope I managed to convey the reason I've been less mentally available recently. Things are gonna stay ugly for me for most of this month but I know we will have something cool at the end of this.

Have a great day.

Peace.


[0] funnily enough the old one is still in there right now being used by the UI. I'll drop it when I get to that part of the rewrite.

[1] there are obvious cases where we dont have to recompute a tile's transform every frame, for example when an animation has finished playing. This is true and will be handled by Spaghet scripts knowing when they can go to 'sleep'. I'll cover this in another post.
