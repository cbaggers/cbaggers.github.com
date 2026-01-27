---
layout: post
title: TaleSpire Dev Log 493
description:
date: 2026-01-26 16:55:08
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Today I worked on updating a bunch of our libraries to support a feature in Unity: Disabled domain reload

You can find out the official details [here](https://docs.unity3d.com/2022.3/Documentation/Manual/DomainReloading.html). It does a good job at explaining it actually, so I'll quote it here:

> Domain Reloading resets your scripting state, and is enabled by default. It provides you with a completely fresh scripting state, and resets all static fields and registered handlers each time you enter Play Mode. This means each time you enter Play Mode in the Unity Editor, your Project begins playing in a very similar way to when it first starts up in a build.
>
> Domain Reloading takes time, and this time increases with the number and complexity of the scripts
> in your Project. When it takes a long time to enter Play Mode, it becomes harder to rapidly iterate on your Project. This is the reason Unity provides the option to turn off Domain Reloading.

It doesn't affect you folks directly, as it's purely a Unity Editor thing, BUT being able to iterate faster will be huge for us, so it's worth the investment.

It took all day, but in the project where I am developing the particle system, the time to enter play-mode went from over six seconds to under one! I'll take it.

Learning this has been very useful so now I can dive into writing code to run the effects in edit-mode with more certainty.

Until tomorrow,

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
