---
layout: post
title: TaleSpire Dev Log 117
description:
date: 2019-07-09 02:50:48
category:
tags: ['Bouncyrock', 'TaleSpire']
---

A quick update for today.

The day was mostly spent reading up on the new [Unity.Physics package](https://docs.unity3d.com/Packages/com.unity.physics@0.0/manual/index.html) to see if it will be viable for use in TaleSpire. It is a preview package and that does come with a level of risk, however we only use the currently physics system for dice and for casting rays against for tools. This means our requirements are very simple and even at it's current state the package has what we would need, assuming of course that it works as documented.

It certainly has some surprising aspects such as how it's not [framerate independent by default](https://forum.unity.com/threads/issues-with-physics-velocity-in-ecs.692953/), but handling that seems to be easy enough.

Peace
