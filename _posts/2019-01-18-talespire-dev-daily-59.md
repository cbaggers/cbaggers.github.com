---
layout: post
title: TaleSpire Dev Daily 59
description:
date: 2019-01-17 23:19:49
category:
tags: ['BouncyRock', 'TaleSpire']
---

Hey again, it's been a satisfying day. I spent the morning tracing through the logic that handles creature ownership and some users have seen a bug where people lose control of unique creatures seemingly randomly. By doing that I found one mistake that was causing 2 bugs:

- a unique creature deleted and respawned from the unique creature panel has incorrect ownership
- making a creature unique gives the GM permissions to the unique on the server even if they didn't have that previously have that client side.

Luckily the bugs came from human error rather than something fundamentally wrong with the design of the system, so that was nice. I also managed to consistently reproduce another bug:

- If a player joins a room that already contains a unique creature they should have permission to, it doesnt give them that permission.

I expect this is a mistake is how we setup the synchronized version of the board.

I think tomorrow I should be able to get that bug fixed with plenty of time to spare. With the rest of the I want to go after a more fleeting and thus trickier bug. I have seen cases where I have dragged in a unique and all seems to be fine, but then the unique disappears. It almost looks like twhat happens when a unique is summoned to another board so I'll probably start debugging there and see what comes up.

I'd be quiet happy if I can just get a reliable way to reproduce the bug tomorrow. I'll then be in a good place to fix it Monday, or over the weekend if I fancy.

After the month or so of crunch I am really trying to get back in the habit of doing things other than TaleSpire in my free time so I don't burn out. I've been playing ff9 again after a 2 month break and I really want to start looking back into my personal projects again. Maybe next Wednesday I'll restart the lisp streams.

Right, time to get some rest.

Peace.
