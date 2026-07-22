---
layout: post
title: TaleSpire Dev Log 509
description:
date: 2026-05-26 09:51:24
category:
tags: ['Bouncyrock', 'TaleSpire']
---

G'morning folks.

While I wait to see if there are bug reports from the [hide-volume improvements Beta](https://talespire.com/welcome-to-the-hide-tweak-Beta), my focus is back on Physics optimization.

It's very important to us that  TaleSpire runs well on lower-end machines. Not least because the cost of upgrading is, for now, so much higher than normal.

I'm currently working on the Physics front-end. The code that the game interacts with to use the physics engine. The changes allow more of the physics code to run concurrently with other tasks and to replace code compiled using Mono with code that can be Burst-compiled.

On a board with a lot of creatures, a significant amount of time was spent simply writing data into and out of the system because the GameObjects we were using stopped us from using the Burst compiler. I've rewritten all of that code, and I'm confident that those steps will not be an issue in future.

I'll keep on plugging away at this. I'm not loving my old spider'y code, but untangling it is letting me find better ways to get performance out of it.

See ya in the next log!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
