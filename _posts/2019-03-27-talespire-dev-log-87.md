---
layout: post
title: TaleSpire Dev Log 87
description:
date: 2019-03-27 23:04:15
category:
tags: ['TaleSpire', 'BouncyRock']
---

Today was an interesting mix.

I got around the staging server issue from yesterday by using a different subdomain..yeah I'm not happy with it either. I'm guessing that the certs weren't cached and so every time I pushed a new build it was asking letsencrypt for new certs.. they have a limit of how many they will give out in a period of time so maaaaybe that's it. However I was sure I had seen that before and it would error rather than giving something invalid. So I don't know.

The correct thing to do would be to inspect the certs I received and see what was up with them. However this week we are really pressed for time so for now I'll settle for dodging the issue rather than fixing it.

Because of that I was able to test the new 'Check if I should have received an invite' site. It works so, after a few tweaks to make it mobile friendly, we called it done for now. We will make it available to you with the next wave of invites.

Speaking of those we have been way behind on that, the content updates we were wanting to complete first have taken longer to complete as we have started prepping material for the upcoming kickstarter. This is also why we haven't had a mini update this week (booooo :[)

After the above stuff I started doing some work on dice, there are some ownership and syncing tweaks I want to make.

I also added basic per-server request stats to our front end server code. This just means we get per minute info on what requests are made to the server in a format that should be easy to graph in cloudwatch. Super low tech approach but if it works for now it will be a boon.

That's all for today. Back with more tomorrow.

p.s Oh I'm away from this Sunday to next Wednesday as I'm off to a programming conference. There will be a small gap in these dev logs but they'll be back as soon as I am :)
