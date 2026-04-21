---
layout: post
title: TaleSpire Dev Log 499
description:
date: 2026-02-10 23:58:28
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Wait.. I already did this today. I wrote a dev-log, and then told myself, “Alright, I’ll just code this one thing and then post it”... and now it’s midnight. So, just before my original dev-log here is the one from today:

I worked on exactly the kind of polish I mention below, with lots of focus on how the system communicates errors to the user.

I’ll leave it at that as there’s already a whole post to read.

Bye!



Heya folks,

Work continues well on the VFX-System[0]. Over Friday and Monday, I got the effects running in Unity's edit-mode instead of just play-mode. This makes it way easier to iterate without having to start the game every time. It took a little extra prodding to get it behaving well in prefab-scenes too, but that also works as expected.

Along with the usual bug fixes, I also got to work on little details that make the experience nicer for the artist. Nicer defaults in new assets, right-click menu options for easily adding effects to scenes, pre-configured effects so things are immediately visible when you create them. And so on.

Today I'm going to continue working on polish, and then I'll dive back into effects that procedurally generate geometry, instead of just drawing meshes or billboards. We had that working in an earlier build, so I don't foresee that being difficult.

After that, it's all about fleshing things out! I'm hoping to hand this over to Ree later this month. Then, as they explore what Alex and I built, I'll get stuck into the next project (the system architecture improvements I mentioned in a previous log).

Today is one of those wonderful days where the road, while long, seems clear. TaleSpire is a lovely project to work on, and it's going to get so much better.

I hope inspiration finds you today too.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] I've been calling it a particle system, as the name was more familiar. However, in our current terminology, the top-level concept is an 'Effect'. An effect is a container for multiple 'Particle Systems'. Each particle-system updates separately, but each particle can add new particles to any of the other systems (amongst other things).
