---
layout: post
title: TaleSpire Dev Log 98
description:
date: 2019-04-16 18:51:21
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today has been fun.

I fixed a bug that appeared when editing the lineup in initiative mode. Each creature added would recreate the whole list which, whilst it technically worked, it looked bad. This was a simple 'reused the elements which were there previously' fix. So that was nice.

One of our lovely gang in discord then pointed out that when you first spawn dice they have a nice spin which is an effective randomizer, but when you pick up rolled dice they don't spin. Now then you throw we do add some random angular momentum so the result should still be random, however it didn't feel great so I've added the spin on pickup too.

Lastly I've done most of the work to add a way to control the volume of sound effects in the same fashion as music and ambiance. I'm still hunting down a bug where this new volume is not applied correctly but it's nearly done.

My goal for tomorrow is to get an update out so if I don't get the effect volume stuff done in time it's no big deal, we'll probably have another update at the weekend anyway.

Peace folks.

![spin](/assets/videos/pickupSpin0.gif)
