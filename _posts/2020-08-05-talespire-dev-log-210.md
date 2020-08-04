---
layout: post
title: TaleSpire Dev Log 210
description:
date: 2020-08-05 01:47:05
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks, I skipped yesterday's log as I was far too deep in code, and to be honest today has been much the same.

The TLDR is that I'm working on TaleWeaver and the modding tools. I'm doing this as we want to move to more directly managing the batching of what is drawn rather than leaving it all to Unity. Changing the way that works requires different data, a lot of which can be packed into the asset when it is exported from TaleWeaver. Changing this batching approach also means we need our own animation system, which is fine but related to the scripting system (Spaghet). Before you know it, we have major changes to scripting, animation, rendering, and more. 

It's been a heck of a lot to keep in my head at once. Every decision impacts how another system works. Complicating this, of course, is this should be at least reasonably performant, so you cant just use <insert your favorite design-pattern/paradigm/etc> and trust that it will come out rosy :P

All in all, I've just not had enough bandwidth to tackle anything else. 

Ree has also been busy in the experiments around props. I'm already very excited to see what comes out of that.

Tomorrow I need to take some hours to look into some networking issues some users are having. Sorry for not getting to you sooner, you know who you are.

Warm regards from code-land.
