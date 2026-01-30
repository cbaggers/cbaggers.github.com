---
layout: post
title: TaleSpire Dev Log 494
description:
date: 2026-01-29 11:29:31
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Allo folks,

This dev-log is for the last two days

I’m working on the code that runs an "effect" as defined in our particle system. This code is currently called the "effect-driver" which may change, but will do for this log.

The effect-driver is a MonoBehaviour (a component that lives on Unity's GameObjects and gives them behaviour) that holds a reference to an effect made in our system. It then does three main things:

1. Dispatches the various compute-shaders that updates the particles, and then the code that draws them

2. Notices any changes to the definition of the effect and updates or recreates the buffers used for the effect

3. Provides a Unity-friendly UX to the folks configuring the effects. This means both that it’s associated with a GameObject and that it provides UI in the inspector to configure the values used by the effect.

This is already a bunch of work, but there are also other concerns. Unlike normal game code, we need the effect to run in edit-mode rather than just play-mode. This is the norm for tools like these, it would suck to have to run the game every time you wanted to tweak how a particle effect would behave.

There is a tool for this, [ExecuteAlways](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/ExecuteAlways.html). I’m getting used to how this works. Along with disabling domain-reload (which I mentioned in the last log), this does make fairly major changes to the normal flow of a MonoBehaviour’s lifecycle, so I’m having to read a lot to make sure I have actually understood what it’s doing.

Progress is steady though. I am currently working on the code that works out what parts of an effect have changes and reconfigures the driver.

Until next time.

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
