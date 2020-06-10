---
layout: post
title: TaleSprie Dev Log 188
description:
date: 2020-06-09 02:02:37
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Evening all.

Work goes well. @Ree has been looking back into the emote system work he did previously. Once that is up to scratch, we can get the data & sync side worked out, and then hopefully, it's just testing and tweaks before we ship.

Today I started with a few hours of going through all the posts in the #feature-request channel in discord, making the big list of things to discuss internally. Starting on Wednesday, @Ree and I are taking some days to plan and hack on some experiments to set the direction for what we need to achieve over the coming year.

Inspired by an item on that list, I've been revisiting [my old NDI tests](https://bouncyrock.com/news/articles/talespire-dev-log-119) to see how much work it would take to get this shipped experimentally. I can pick the source from NDI and get texture, but I'm currently fighting the UI side. Once that is doing what I want, I'll need to handle some strangeness I saw when the call reconnects before looking at sync. I'm not yet sure how the sources are named on each client, so I'm not sure how to synchronize the feed information for each client. Regardless we'll work it out or, if it takes too long, it'll go back on the shelf for now.

That's all for today. I'll put out a bugfix tomorrow as today I was made aware of a bug that was introduced to line-of-sight in the last release.

Goodnight!
