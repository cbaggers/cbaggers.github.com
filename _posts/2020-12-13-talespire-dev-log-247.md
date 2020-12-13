---
layout: post
title: TaleSpire Dev Log 247
description:
date: 2020-12-13 07:25:55
category:
tags: ['Bouncyrock', 'TaleSpire']
---

'Allo all!

I'm on a high right now as I just fixed a bug thats been worrying us for a while.

TLDR: On our new tech branch of TaleSpire, physics is now stable at low framerates

Now for the director's cut!

We have been working hard for a while on a big rewrite of the codebase, which gives significant performance improvements. Amongst the changes was a switch to Unity's new physics engine (DotsPhysics). For the dynamic objects in the game, we still wanted to use GameObjects, so I made a wrapper around DotsPhysics, which made this feel very similar to the old system.

It's been working well enough since the last [batch of fixes](https://bouncyrock.com/news/articles/talespire-dev-log-233); however, we had noticed that it got very unstable at low framerates. I was nervous about this as, if it wasn't my fault, I'd have no idea how to fix it (Spoiler: It was totally my fault :P)

It couldn't be avoided for much longer, though, so for the last couple of days, I've been looking into it. Yesterday was exceedingly painful. I read and re-read the integration code Unity use for their ECS, and simply couldn't find anywhere where we should have been messing up. I tried a slew of things with no results. That day ended on a low note for sure.

Today however, I started fresh. For the sake of the explanation, let's assume the code looks roughly like this:

```
Update()
{
    RunPhysics();
}

RunPhysics()
{
    LoadDataIntoPhysicsEngine();

    while(haveMoreStepsToRun)
    {
        RunPhysicsStep();
    }

    ReadDataBackOutOfPhysicsEngine();
}
```

I had noticed yesterday that the first physics step of every frame worked fine. So I knew it had to be related to how I was handling the [fixed timestep](https://gafferongames.com/post/fix_your_timestep/).

So I changed the code to look like this.

```
Update()
{
    RunPhysics();
    RunPhysics();
}

RunPhysics()
{
    LoadDataIntoPhysicsEngine();

    // while(haveMoreStepsToRun)
    {
        RunPhysicsStep();
    }

    ReadDataBackOutOfPhysicsEngine();
}
```

And low and behold, it was still stable. The speed was totally incorrect, of course, but it was clear that `RunPhysicsStep` was missing something that was handled in the setup or teardown code.

I made small change after small change and finally was able to isolate the issue. One big difference between my code and how Unity was doing things was that they read out the data from the physics engine at the end of each step and then read it back in at the start of each step. This is desirable for them, but it was not something I was doing as for our use-case, it was just overhead. However, what I had forgotten was that when we load the data into the physics engine's data structures, there are two places you have to put the transform for the bodies being simulated. After each physics step, only one has the updated transform[0], so I needed to make sure that the transform was written back to the other.

And that was it! Suddenly the simulation is solid as a rock at 20fps, and I could breathe a sigh of relief.

I'm now going to do some performance tests again, tweak my fixed-timestep implementation, and then move on to rewriting our board representation to use our new [light implementation](https://bouncyrock.com/news/articles/talespire-dev-log-243).

Hope you have a great day,
Seeya!


[0] This makes sense when you look at it and is not an issue with how Unity made this.
