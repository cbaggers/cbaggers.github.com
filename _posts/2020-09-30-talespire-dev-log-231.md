---
layout: post
title: TaleSpire Dev Log 231
description:
date: 2020-09-30 02:55:33
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks, I was meant to write this in the morning, but code is distracting :P

Yesterday I worked on dice physics. I had been concerned by the behavior of dice and wanted to find a good path forward.

To be able to see what was going on, I made this test scene. I can reset the dice by clicking the reset 'button.'

![goofy test scene](/assets/videos/physicsGoofyTest.gif)

This is one place where Unity absolutely excels. Being able to knock up tests and tooling in minutes so often can help you make headway on otherwise stubborn issues.

There are a few points that I find interesting:

DotsPhysics and ClassicPhysics do not line up in behavior. This is really important as this means that I shouldn't expect my conversion routines to give the same results as classic as mine are directly based on the ones built into DotsPhysics.

The BouncePhysics wrapper around DotsPhysics lines up with the behavior of DotsPhysics. In that, the way they bounce and come to rest is similar. This lends support to the point above and gives some evidence that the conversion is ok.

DotsPhysics falls faster. This is because they don't handle time properly, so faster framerate means faster simulation.

Mine falls at the same speed as ClassicPhysics. This is very important. It suggests my fixed-timestep code and use of Unity's simulations values (like gravity) are in the right ballpark.

Once I realized that DotsPhysics doesn't give the same bounce, drag, etc, for the same input values, I felt comfortable playing with values to find something that felt right. This clip is not final behavior but shows how a couple of minutes of tweaking can help.

![a little better](/assets/videos/physicsGoofyTest2.gif)

Afterward, I set up some ramps and D12s, so we had a decent place to start testing when we sit down to dial this in for real.

I also saw that my interpolation code was incorrect. I found some issues, but I was struggling to match all of Unity's behavior. I then noticed that we don't use interpolation in the current build and so this stopped being a priority. However, it still took me a couple of hours to quit playing with it. It's so tempting to keep going when you know you have the math right, and it's the surrounding plumbing that is the issue.

Anyhoo that was that. Today I've been working on the character controller again, results are promising, but I'll leave that until tomorrow.

Goodnight
