---
layout: post
title: TaleSpire Dev Log 247
description:
date: 2020-12-13 07:25:55
category:
tags: ['Bouncyrock', 'TaleSpire']
---

'Allo all!

On a bit of a high right now as just fixed a bug that had been worrying us. 

TLDR: On our new tech branch of TaleSpire physics is now stable at low framerates

Now for the longer version.

We have been working hard for a while on a big rewrite of the codebase which give significant performance improvements. Amongst the changes was a switch to Unity's new physics engine (DotsPhysics). For the dynamic objects in the game we still wanted to use GameObjects so I made a wrapper around DotsPhysics which main this feel very similar to the old system.

It's been working well enough since the last [batch of fixes](https://bouncyrock.com/news/articles/talespire-dev-log-233), however we had noticed that it got very unstable at low framerates. I was nervous about this as, if it wasnt my fault, I'd have no idea how to fix it (Spoiler: It was totally my fault :P)

It couldn't be avoided for much longer though os for the last couple of days I've been looking into it. Yesterday was exceedingly painful. I read and re-read the integration code Unity use for their ECS and simply couldnt find anywhere where we should have been messing up. I tried a slew of things with no results. That day ended on a low note for sure.

Today however I started fresh. For the sake of the explanation let's assume the code looks roughly like this:

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

So I changed the code to look like this

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

And low and behold it was still stable. The speed was totally incorrect of course but it was clean that `RunPhysicsStep` was missing something that was handled in the setup or teardown code.

I made small change after small change and finally was able to isolate the issue. One big difference between my code and how Unity were doing things was that they read out the data from the physics engine at the end of each step and then read it back in at the start of each step. This is desirable for them but it was not something I was doing as for our usecase it was just overhead. However what I had forgotten was that, when we load the data into the physics-engine's datastructures, there are two places you have to put the transform for the bodies being simulated. After each physics step only one has the updated transform[0] so I needed to make sure that transform was written back to the other.

And that was it! Suddenly the simulation is solid as a rock at 20fps and I could breath a sigh of relief.

I'm now going to do some performance tests again, tweak my fixed-timestep implementation, and then move on to rewriting our board representation to use our new [light implementation](https://bouncyrock.com/news/articles/talespire-dev-log-243).

Hope you have a great day,
Seeya!



[0] This makes sense when you look at it and is not an issues with how Unity made this.
