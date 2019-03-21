---
layout: post
title: TaleSpire Dev Log 84
description:
date: 2019-03-21 22:21:42
category:
tags: ['BouncyRock', 'TaleSpire']
---

Turns out that the issues with the server images was simple after all. 

Back in january I tweaked the images to include the amazon agent so we could get memory usage info in cloudwatch. When I did that I must have left some files from an old build around. Later on, when the newer code was pulled and extracted, it didnt replace what was already there and we ended up running this weird frankenbuild when we had old code and a new db schema.

Dumb but fine. I then fixed up the server side code for storing the default board. I'll push that to live tomorrow and then soon we'll work on ingame UI for this. I expect we'll add a marker to the entries in the board panel.

Ok, time to sign off.
Peace
