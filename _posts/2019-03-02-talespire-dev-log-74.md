---
layout: post
title: TaleSpire Dev Log 74
description:
date: 2019-03-02 00:53:08
category:
tags: ['BouncyRock', 'TaleSpire']
---

It's late but I don't want to skip this today so here we go.

Today I've mainly been focusing on bugs relating to the order of creatures in the battle-mode (also called initiative-mode) lineup. One such issue can be found [here](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/88).

The main one from today was fixing the behavior when the currently active creature is deleted as control passed not being passed to the next in the list. I fixed that but I have seen a number of issues and I'll need to test those pretty thoroughly before I'm happy to push this out. This code feels like a place where it is very easy to introduce regressions.

I'm hoping I'll get the majority of the related issues cleaned up on Monday so we can push another update asap.

We actually have a bunch of asset changes too so we'd like to get those out soon too. These changes flesh out the various asset collections to match the tiles available in the test tile set.

Time to sleep, gotta get up early to watch the SpaceX RD1 launch tomorrow morning :)

Peace.
