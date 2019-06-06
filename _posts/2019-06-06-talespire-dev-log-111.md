---
layout: post
title: TaleSpire Dev Log 111
description:
date: 2019-06-06 11:40:33
category:
tags: ['BouncyRock', 'TaleSpire']
---

I was too tired to writeup last night's dev log so here it is a little late.

I really want to get a user visible feature out this week so I implemented 'duplicate board'. This will let you click on an icon in the board panel and duplicate the chosen board. This will be handy for a lot of cases but I love the idea of making a town, copying it and then destroying the town for the players to visit again *after the calamity*.

The hard parts had been researched or implemented when I was looking into the S3 part of cleaning up old board files. All that was left to do was write the database queries and expose it to the game itself.

With the core feature now working I just need to do cleanup and handle the fact that it currently doesnt copy the board description (an easy fix).

Back later today with more news
