---
layout: post
title: TaleSpire Dev Log 329
description:
date: 2022-03-09 16:46:38
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Yesterday the code gods conspired against us, so we didn't get to start on the latest iteration of the HeroForge mini specification.

Instead, I focused on packages.

We use a bunch of experimental packages, and naturally, these have bugs from time to time. Yesterday we had such a case with `Unity.Entities`. The package had already been fixed in the latest update, but that fix was incompatible with the latest version of another package with its own bug  (and so on it goes).

This led to me shuffling around packages versions until I got a setup that will work for now.

The amusing part is that we will be removing the Entities package soon anyway. We currently have it because:

1. Unity.Physics depends on it
2. We use a couple of serialization tools that happen to live in that package.

Neither of these reasons will be valid for long, however. We are forking `Unity.Physics` to optimize it for TaleSpire's specific use case. As we don't use Unity's ECS, we don't need the ECS integration in the physics engine.[0]

As for Serialization, I have already started working on internal tooling for defining data formats and convertors between them. This will help in a few ways:

- It will reduce the amount of hard to maintain serialization code.
- It will make writing & upgrading binary formats more approachable to more of the team.
- It should make it easier to iterate on the formats used in mods without worrying about breaking what is already out there.

I only need the basics of this to allow us to drop `Unity.Entities`. That, in turn, will let us to pull the latest version of the conflicting packages and put us in a great place to improve the performance of physics.

And that, for today, is that!

Seeya you around.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] In fact, I did this today! It's sitting on a branch, ready to merge
