---
layout: post
title: TaleSpire Dev Daily 47
description:
date: 2018-12-06 00:03:13
category:
tags: ['BouncyRock', 'TaleSpire']
---

Some serious yak shaving has been taking place today.

The goal was to add the 'manager' that would keep track of the connection to the backend and handle cases where we lose connection. One task was to be able to inform a request if it should retry, but hmm.. that means I should really refactor the queries a little.

With my new goal I dug into the queries a bit and saw that I should really take care of the changes to board I needed to make first. We want to be able to keep multiple versions of the board on the server so we have a backup history of sorts. This requires some work on the database.

With my new goal I started writing the board changes and saw that a bunch of this code will need revisiting anyway as we expose some guids we use as keys and that it's a good plan. So I made the basic changes and then switched task.

With my new goal..well it's a monster. The guids were used in a tonne of places and so about 70% of the queries in the backend needed to be refactored. This has taken me the rest of the day but, as it's work we needed to do for the release anyway, I'm not worried.

I'm not at the point where I have pushed a new staging build and am testing TaleSpire (spoiler there are still bugs). There are a good few hours of work before this is working properly again and I'm not sure how much I will get done tomorrow as I need to visit the doctor. We shall see anyway.

Until next time,

Peace
