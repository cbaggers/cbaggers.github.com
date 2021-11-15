---
layout: post
title: TaleSpire Dev Log 299
description:
date: 2021-11-15 03:57:01
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

Before we get started, the number of the dev-logs is not something we sync up with features or other announcements. We aren't that media-savvy :P

## Rendering nerdage

Ok, let's get into things. In my last log, I was working away on HeroForge integration. However, on Thursday, I [nerd sniped](https://xkcd.com/356/) myself by watching [A Deep Dive into Nanite Virtualized Geometry](https://www.youtube.com/watch?v=eviSykqSUUw&t=519s) from Unreal and finally understanding some details of occlusion culling. It was just a little detail about building the early Hi-Z, but it got my mind wiring.

This, in turn, spun me off to this [phenominal talk on clustered shading and shadows](https://www.youtube.com/watch?v=uEtI7JRBVXk) by Ola Olsson[0]. Even though we've managed to replicate Unity's lighting approach without GameObjects, it simply doesn't handle thousands of lights efficiently. This feels like something we should be able to integrate with Unity's built-in rendering pipeline to improve non-shadow-casting lights.

On the subject of the built-in rendering pipeline, I should take this quick detour. The built-in rendering pipeline (sometimes called the BIRP) is the rendering pipeline that is enabled out of the box in Unity. They have, much more recently, added the SRP (scriptable rendering pipeline) that exposes much more control to developers. However:

- Porting a project to these is non-trivial and would require a lot of engineering and work on both TaleSpire and TaleWeaver
- They are not finished. In fact, SRP has been on hold for a while now as Unity works on the internals of DOTS and which we won't go into here[1]

Because of this, we are looking at sticking with the BIRP until we simply outgrow it and have to invest in SRP. The good news is that it's looking like we might be able to do more than I expected before having to take that plunge.

Ok back to code.

With the clustered shading talk fresh in my mind, I looked around to find more info about the technique. In the process, I found [this post](http://www.aortiz.me/2018/12/21/CG.html#determining-active-clusters), which was a great read on clustering in itself and, I'm sure, will be very valuable in the future.

While musing back on the occlusion culling, it got me thinking about early-z passes in general, and I found this [post](https://interplayoflight.wordpress.com/2020/12/21/to-z-prepass-or-not-to-z-prepass/). I don't have anything to add about it, but it sparked plenty of ideas off in my head.

While the lights on our tiles and props currently don't cast shadows, the sun, on the other hand, does, and rendering it is a significant portion of frame-time. Even if we could make our own version and use occlusion culling to optimize the process, I didn't think we could integrate it without SRP [2].

After reading these two [excellent](https://shahriyarshahrabi.medium.com/custom-shadow-mapping-in-unity-c42a81e1bbf8) [articles](https://catlikecoding.com/unity/tutorials/rendering/part-7/) on how Unity handles its cascaded shadow maps, I spotted something that might allow us to hook our own approach in. As shown in cutlikecoding's article, Unity collects the shadows into a screen-space map after making the cascades. Not only is the shader that does that [available](https://github.com/TwoTailsGames/Unity-Built-in-Shaders/blob/master/DefaultResourcesExtra/Internal-ScreenSpaceShadows.shader) [2] but Unity also gives an option in the project settings to replace it.

![ui to replace the shader](/assets/images/unityShaderStages.png)

I'm hoping that this means we could make a version of this that integrates both the Unity shadow map and our own.

This is, of course, speculation at this point. But it was rather exciting nevertheless.

## Performance

The above research included plenty I haven't mentioned, and Friday had come to a close when I wrapped that up.

However, I was in a performance frame of mind, and I had a hankering to see what I could improve. To this end, I broke out the profiler and went looking.

### Physics

One experiment I wanted to do was on how we use Unity's `Unity.Physics` engine[3]. The engine is considered stateless, meaning you have to load in all the rigid-bodies you want to include in the simulation, each frame. There is an optimization, however, where you can tell it that the static objects are the same as last time, and thus it doesn't have to rebuild the BVH (bounding volume hierarchy) or other internal data for statics.

As you can imagine, in a board with hundreds of thousands of tiles, loading that data into the physics engine starts taking a non-trivial amount of time.

The test board I was using was a [monster that was shared on the discord](https://discord.com/channels/262552432968466432/575333285719310376/836588588900548700) and has many thousands of zones. We spawned a job for each copy, and I was concerned that the overhead was hurting the speed. To that end, I gathered all the pointers and lengths into an array and spawned fewer jobs to do the copying. It didn't make a big difference, however. What did, was calling `JobHandle.ScheduleBatchedJobs` after scheduling every N jobs. It kept the workers fed well and gave enough of a speedup that I moved on to other things.

The next thing I wanted to test was, very crudely, would only copying the data from nearby zones be a win, given that the physics engine would have to rebuild the internal data for the statics more often? I quickly hacked this in and, to get a nice worst case, I forced the static rebuild on every frame.

I was, as expected, able to see an improvement, even with the rebuild costs. What I rediscovered, though, was how nice double-right clicking is to zip around the board. I noticed this because I wasn't including zones from far away, so I couldn't click them. This is not a show stopped by any means, but it is something I'd need to keep in mind when making a proper solution.

The proper solution would need to work out which zones need physics[4], and then minimize the number of times we need to rebuild the statics by being smart about when to drop zones from simulation.

During all this, I finally spotted something which had been bugging me for ages. I had never understood why we needed to copy the statics in every frame when, as far as I could see, they weren't being modified. On re-reading their code, I finally noticed that they store dynamic and static rigid-bodies in the same array and that statics always came after dynamics. This meant that the indices from the BVH to the static bodies became incorrect whenever the number of dynamic rigid-bodies changed.

The upshot of this is that we can skip copying static objects if the zones have not been modified and the number of dynamic bodies has not changed since the last frame.

This gave a noticeable improvement! In the future, we'd probably want to fork their code and lay the data out in a way that lets us avoid copying more often. I'm also curious if we could develop a way to rebuild portions of the static data instead of all of it. Very exciting if possible.

### General optimization

I remember an aphorism that goes roughly:

> If you get a speedup of 100 or 1000 times, you probably didn't start doing something smart, but instead stopped doing something dumb.

While we are definitely not seeing anything in the realm of 100 times speedups, I think we are still well in the land of 'stopping doing dumb things' when it comes to performance.

Over the course of the weekend, I did the following:

- Added an early out to culling when we are given a frustum which will definitely cull all assets
- Changed all Spaghet managers to share the 'globals' data for scripts. This meant we only had to update one.
- Noticed that zones checked every frame to see if they had missing assets that had finished loading. The zones now register for updates and unregister once all are loaded.
- Learned that calling [GetNativeBufferPtr](https://docs.unity3d.com/ScriptReference/ComputeBuffer.GetNativeBufferPtr.html) causes a synchronization with the rendering thread and that we should cache it instead.
- Refactored a bunch of update logic zones push data into collections for use in the next frame rather than scanning the zones to collect it
- Started using culling info to avoid uploading data for dynamic lights.

And other little things.

The result is that I was able to get a 50% improvement in framerate in my test on the monster board.

![fps improvment](/assets/images/aLittleBitBetter.png)

Please note: This does not mean 50% improvement in all boards. This is a result of a single test.

The speedups are probably more noticeable in large boards. However, even small improvements in smaller boards are worth it.[5]


## Linux

Since our [announcement that we would support macOS and Linux](https://bouncyrock.com/news/articles/the-road-to-multiplatform), I've been trying to get info from Steam regarding how best to ship with Proton.

For context, and as [itsfoss explains](https://itsfoss.com/steam-play/), on Linux, you can choose to enable SteamPlay for 'supported titles' or 'all other titles'.

We inquired about two things:

- How to be a supported title
- Whether we could *pin* a particular proton version as default so we could test against a known software

There has been a lot of back and forth over the last month, but I think we have the info now.

As to the version pinning question, the answer was no.

As to the first question, we got this feedback from support:

> No problem, Chris! It's a confusing piece slightly because of past history.
>
> Before SteamDeck drove a bunch of additional effort on Proton, games did need to be "whitelisted" so to speak, and a customer playing games on Linux could take the step to say "let me try Windows games via proton even if they haven't been whitelisted"
>
> So, those customer-facing settings in the Steam client are still visible. But because we've made so much progress with Proton, the notion of that whitelisting doesn't really make sense-- we don't update it, and we may phase it out of the Steam Linux client settings altogether at some point.
>
> For you as a developer, there's no longer a list to be added to-- you're good to go!

This is good and... well, not bad but interesting. We would like to make TaleSpire on Linux as obviously supported as on Windows. However, it seems like it will be behind an option for now, and we don't have a way to indicate when we officially support it.

It might be a non-issue, as we Linux folks are used to having to jump through some hoops, but it is a little disappointing.

One option would be to bundle Proton with TaleSpire ourselves and ship it as a Linux native game. This would then appear in the store as such. This is a bunch more work, and feedback has indicated that it would still be good to allow people to use their own proton install if desired.

It's an interesting situation, to be sure. We are still a little ways off from looking into the glaring Linux-specific bugs, so we don't have to worry about an official release yet.

We'll definitely keep you posted.

## Wrapping up

That's enough for today. Congrats if you made it all the way to the end!

Hope you have a lovely week, and I'll be back soon with more dev-logs

Ciao.



[0] The lighting in the video isn't great, and so I recommend following allow with the slides you can find here https://efficientshading.com/2016/07/12/game-technology-brisbane-presentation-2016/

[1] Not least because there is so little public information about it, and far too much conjecture online.

[2] Officially available from Unity Hub or [here](https://unity3d.com/get-unity/download/archive)

[3] This is not the default physics engine in Unity but one which shipped with DOTs. It is stateless and impressively fast given what we throw at it.

[4] These places include:
- wherever a player is pointing with their cursor
- wherever any dice are
- wherever any moving creatures are
- etc

[5] Physics really shows this to be true. The longer a frame takes, the more time the physics engine has to simulate the next frame, and simulating more time takes more time. This means that speeding up non-physics code can mean the physics is using less time per frame.
