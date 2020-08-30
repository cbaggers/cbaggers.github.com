---
layout: post
title: TaleSpire Dev Log 220
description:
date: 2020-08-29 16:05:37
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Phew, a few days away from the dev-log there. I got really caught up in some chnages that were a real grind. 

I've been working on the behaviour of uncommitted changes. This work is taxing as you have a matrix of different states tiles might be in. Let's take redo of delete for an example.

- You might be redoing an undo which has been committed
- You might be redoing an undo which has not yet been committed but the delete action it undoes has been committed
- You might be redoing an undo where niether the undo or redo have been committed yet

And that's one action in isolation. What about the delete of assets which were made by undoing a seperate delete. The matrix of possibilities applies to all the actions involved in that sentance too. In short making sure you get a reasonable result takes a lot of concentration (and bizarre diagrams :D ).

When I finally got it running locally well I connected up a second client and immediately disaster. The result were very wrong. I'll admit I started panic sweating at that point. Last time this happened we had to delay the Beta. Luckily this time the system is much simpler and I have an extensive test suite to lean on. 

I dusted off the tests (I may have neglected them for a while) and started digging into issues. The great news was that, whilst the tests did find a couple of bugs in the new code, the underlying board code was still working fine.

This helped me relax, and focused my attention back into the code handling the batching of uncommitted tiles. After some time in the debugger I finally spotted a case where tiles were not removed from the uncommitted state when committed. This ammounted to a constant being 1 instead of -1. With that typo fixed it started behaving rather well.

With that terrifying detour done, I needed to get back into physics. I have had to write a lot of the physics code fairly reactivley rather than with a solid plan and so I hit the point where I needed to clean things up. This sucked. It was a painful combination of being very teious but also critical to get right. Some days are just like this. 

These last weeks I've really been feeling the pain of not being able to just rely on the game-engine in the ways we usually do. Because of our bad estimate of when Unity's new ECS would be ready we are having to make a lot of systems ourselves. This is fine in theory (I love making stuff) but it's meant losing at least a month of dev time that we did not expect. That's always difficult but our timeline was already tight. It's pretty stressful.

I guess why it's especially on my mind right now is that the biggest bits of the rewrites are working, which means we not get to all the little details, and this is where you really feel how much work has been put into products like Unity.

Anyway, panic/rant aside things are progressing. The thing on my plate now is hooking regular Unity GameObjects into our physics setup. We need something that takes current-gen Unity Physics components and makes their equivalent in a format suitable their new physics engine. We can then write some code to manage copying the values to and from the new physics engine to keep it all in sync.

If all that goes well then hopefully I'll be rolling dice again soon!

Thanks for stopping by folks,

Seeya in the next dev-log
