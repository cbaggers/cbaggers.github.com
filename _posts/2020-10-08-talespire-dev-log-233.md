---
layout: post
title: TaleSpire Dev Log 233
description:
date: 2020-10-08 02:23:32
category:
tags: ['Bouncyrock', 'TaleSpire']
---

![popcord dice bug](/assets/videos/popcornDice.gif)

Internally we have been calling this bug "popcorn dice", and it's been driving me crazy for a while.

As we have our own wrapper around Unity's new physics engine, I had assumed that I was just 'driving' it incorrectly. So today, I sat down and read how Unity's ECS hooks into the physics. I had hoped the mistake was how I was loading in the physics-body data, or when reading it out after the physics step. Alas, no such luck.

I saw that they kept track of their fixed steps in a slightly different way than I did. It should have resulted in identical behavior, but just to be sure, I followed their example. Again no dice (har har).

I read that damn code forwards and backward and finally remembered that I had a test scene that had been surprisingly stable this whole time. It's the one I showed in an update a few weeks back. It looked like this:

So what could this possibly have in common with the ECS tests I'd been doing, but that was also different from the main board scene. I suddenly got a hunch and couldn't help but laugh when it proved to be correct.

The floor was too big.

YUP. So we'd been lazy and just made the board 6000x6000 units in size a looooong time ago and promptly forgot about it. The classic physics engine (bless its soul) had been doing a great job, and so we'd never seen the issue. However, with the new system, if you have mesh colliders interacting with a single box collider more than ~2000 units across, it gets very unstable and starts popcorning like we see above.

This find is a huge relief. TaleSpire obviously relies on the dice behavior being reliable and, if it were a problem with the engine itself, I'd have no idea of how to fix it. This should be trivial. We can make a smaller collider and just teleport it around where we need it.

Alright, that's a great place to end the day.

I'll seeya all tomorrow with something else!
