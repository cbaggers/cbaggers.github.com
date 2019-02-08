---
layout: post
title: TaleSpire Dev Log 61
description:
date: 2019-01-29 22:04:06
category:
tags: ['BouncyRock', 'TaleSpire']
---

I've changed the name until I get my arse back in gear and start doing these daily again.

The weekend was rather eventful. We had a collection of bugs that acted together to corrupt peoples boards. The good news is that a while back we started making checkpoint saves of boards on each session so we are able to revert to those un-borked version.

The principle bug was a mistake is some of the serialization code that made an assumption that the number of Assets in a Tile didn't change. This was a very dumb assumption and it was really just luck that this hadn't bitten us sooner. This was compounded by the fact that, on failing to load, the game didn't recognize this and jump back to the empty board. No, instead it decided the board was out of date and saved the now broken board back to the server :|

Needless to say that changed my Sunday plans. Thanks to some invaluable help from those affected (extra props to Blappo here) we were able to replicate the issue and get a patch out a few hours later. I'm currently working on a proper fix for this but it needs to be done carefully so we don't slip in any more problems.

Also in a couple of days I'll be meeting up with @Ree so we can start work on the next revision of the floor system in TaleSpire. No spoilers today but news (and pics) will come soon.

Alright, time to wrap up,

Seeya
