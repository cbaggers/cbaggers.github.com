---
layout: post
title: TaleSpire Dev Log 221
description:
date: 2020-09-01 14:00:30
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi everyone,

Integrating the new physics engine is going well. I wrote a component that converts old-style colliders and rigid-bodies into the new style.

![conversion](/assets/images/physConversion.png)

> Sorry about the weird colors here. Unity today decided that I last renewed my license tomorrow (yup), and so right now, it thinks I'm unlicensed and gave me the light editor theme[0]. The blue tint on the righthand-side lets me know when I'm in play mode, which is super helpful. It looks a lot less baby-blue when used with the dark theme :P

I'll pretty the UI up in time. For now, seeing everything is helpful.

It took me a while to work out where to get some mass related values right, so the dice rolled strangely, but now it's a little better.

![dice roll](/assets/videos/fixedTimestep.gif)

Behind the scenes, a lot is going on, not least of which is that the physics is running with a fixed-timestep. This shouldn't be a big deal as a fixed-timestep is mandatory for stable physics across framerates. However, this is not [*fully* supported](https://forum.unity.com/threads/fixed-timestep-features-coming-to-entities-dots-physics-packages.899687/) in Unity's ECS, so I was concerned that it would be difficult.

Luckily for us, it was not[1]. The physics engine is pretty great at giving you control, so I loop running the simulation for the required number of steps. If you have read about fixed-timestep before[2] you'll know that you need to interpolate the results as the final step. The physics engine doesn't have that support for that out of the box, so we added a job to collect that information and apply it as we write the transforms back to the GameObjects.

With that done, I need to replicate the parts of the old physics API that we use. If I can get dice rolls working again then I should have replaced most of what we need.

Have a good one folks,
Peace.


[0] yup I do know about the latest version making the dark theme free, but upgrading is risky and we are on a tight schedule right now.

[1] The interpolation is not hard, but the Unity folks have a lot more to support that we do, so we get to only handle the simpler case.

[2] The best known fixed-timestep explanation can be found [over here](https://gafferongames.com/post/fix_your_timestep/). It's a good one.
