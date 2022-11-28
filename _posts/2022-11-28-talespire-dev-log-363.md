---
layout: post
title: TaleSpire Dev Log 363
description:
date: 2022-11-28 00:30:29
category:
tags: ['Bouncyrock', 'TaleSpire']
---

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

Let's look at macOS

Work has continued at a good clip behind the scenes. I've got server updates ready for group move, new vertical controls, and the option to hide the board list from players.

Besides a few bug fixes, I've been working on support for apple silicon macs.

## Apple Silicon

We have started our macOS support focusing on the new apple silicon macs as GPU feature support is spotty across older models, and the push towards the new architecture seems very strong. Steam's hardware survey is already showing that over 49% of macs with Steam installed are apple silicon macs [1].

We are using the m1 iMac as our baseline for apple's new lineup. If we can run well there, we can run well on the rest.


## Final pre-show caveats

I remember an aphorism that goes roughly:

> If you get a speedup of 100 or 1000 times, you probably didn't start doing something smart but instead stopped doing something dumb.

While we are not seeing anything in the realm of 100 times speedups, we are looking at the "stop doing something dumb" category of fixes in this post.

"Dumb" code often arises simply from needing to implement *something* to discover what the feature would become. (But sometimes, it can also just be dumb)

Next, we are going to show timings in some parts of this post. While they were taken from an m1 iMac, they are intended to be purely illustrative. We are only showing timings from single frames and under specific loads. This is not a good representation of the performance in general.

With that said, we hope this is still interesting to some!


## Part 1 - The start point

As I had recently been working on group movement, I decided to use some of the optimizations I had made for the lasso to improve culling for picking single creatures. This benefits all platforms.

I made a board with 600 creatures and got to work.

![](/assets/images/macperf00.jpg)

The picking code requires rendering the creatures into a buffer. With a mild amount of grief, we replaced the old code with one that culls the creatures and only draws the ones whose bounds intersect a ray from the camera.

This gave a 24x improvement for the code that dispatched the creature drawing for the pixel picker. It also reduces the amount of work the GPU needs to do, discarding irrelevant vertices.

![](/assets/images/macperf01.jpg)


## Part 2 - Uploading data

Next, I had this fun one. In the image below, check out the timing differences between the green and red arrows for each platform.

![](/assets/images/macperf02.png)

The green arrows show when we start culling and dispatching render tasks to the render thread.
The red arrows show when the render thread dispatches the render tasks to the GPU.

After a bunch of digging (and convincing XCode to let me profile a frame[2]), I saw that every call to [ComputeBuffer.SetData](https://docs.unity3d.com/ScriptReference/ComputeBuffer.SetData.html) was recreating the buffer. I changed the mode of the `ComputeBuffer` to [SubUpdates](https://docs.unity3d.com/ScriptReference/ComputeBufferMode.SubUpdates.html), which had this effect.

![](/assets/images/macperf03.jpg)

Clearly, this has an effect.

However, with this setting, we can now write over data currently being used, which is not ok. So we have some work to do around here. This leads us neatly to...


## Part 3 - The elephant in the room

I've been working heavily on speeding up CPU tasks. That's all well and good, but the problem we really have is the time it takes to render things.

This is the scene I've been using for testing (ignore the graphical glitches, those are shaders that still need fixing and are unrelated to this case).

![](/assets/images/macPerf04.png)

This is a lot to render. Currently, it's taking over 25ms to do so, which is far too much. 30fps is playable, but we need to dig into how the shaders work if we hope to reach 60fps. XCode's metal frame capture seems pretty temperamental, but I've got some captures we can work with.


## Part 4 - Ignoring the elephant

That is not to say we shouldn't work on the CPU side. By doing so, we can improve all platforms at once.

Given that rendering is always going to take some time, what can we do at the same time? By default, the answer in Unity seems to be "not much." This is because their MonoBehaviours [don't expose a callback](https://docs.unity3d.com/Manual/ExecutionOrder.html) for all interesting points in a frame. However, Unity did add a [low-level API](https://docs.unity3d.com/ScriptReference/LowLevel.PlayerLoop.html) to completely control the system that runs in a frame, which is terrific! [3]

With this new tool in hand, I decided to move the code that applies build changes to the data-model to just after render tasks have been pushed to the render thread.

This meant that, while we still needed to do the work, we hid it behind the work on the render thread.

![](/assets/images/macPerf04.png)


## Part 5 - What elephant?

While I'm pretty happy with hiding work, I was looking at this capture and thinking...

![](/assets/images/macPerf06.png)

... what are these chunky jobs, and why are they taking so long?

The answer was that they are jobs that copy data from dynamic physics objects (like creatures) into the physics system before the simulation runs, and then copies the data back out afterward. The data came from managed objects, so the jobs could not be Burst compiled.

Instead, I moved this data into a native collection that our physics MonoBehaviour could index into when they needed the values. This probably makes main-thread access slightly slower, but this is not where bulk lookups happen.

This allowed for this change.

![](/assets/images/macPerf07.png)

Pretty nice!


## Part 6 - What next

The honest answer is "make the above work" :P

The ComputeBuffer data uploads aren't safe, and the physics changes have some bugs. However, this feels tractable.

On the CPU side, some mesh generation code could be turned into jobs and burst compiled, which could give some notable wins. Also, creatures and dice currently take way too long to update, so I want to look into those too.

Really though, for mac, I need to look at rendering. Any improvements we can make to the shaders would be a big deal, and ideally, we would add LOD support to our batcher. I would love LODs to be a quick win, but because our code had to become so custom, this is not the case. I'll take a look, though.

Anyhoo, that's enough for tonight. I'll try to post a few more dev logs this week, as I have fewer releases to do.

Peace.



[1] A huge caveat for this number is that Steam's hardware survey is guaranteed to be biased due to the chicken-and-egg nature of gaming on mac. Without a big game market, game makers can't afford to focus on mac, and without that focus, the market stays small. (In my opinion) Apple also doesn't seem to care about games outside of gloating at conferences, which makes adoption harder. However, that's a grumble for another day.

[2] No matter how long I work with XCode, it is still exhausting.

[3] You can find some ace articles about the PlayerLoop API here:

- https://medium.com/@thebeardphantom/unity-2018-and-playerloop-5c46a12a677
- https://www.grizzly-machine.com/entries/maximizing-the-benefit-of-c-jobs-using-unitys-new-playerloop-api
- https://www.patreon.com/posts/unity-2018-1-16336053
