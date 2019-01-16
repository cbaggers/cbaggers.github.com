---
layout: post
title: TaleSpire Dev Daily 49
description:
date: 2018-12-10 23:52:19
category:
tags: ['BouncyRock', 'TaleSpire']
---

Hey again, back after a few days without dailies.

Development is still going ok. Jonny has been working hard on getting the new UI to the point where we can merge it into master, looks like we are close there now. Once that lands we can hook things up and write the features that were waiting on that.

I mainly been working on query retries and managing the connection to the backend. I've merged in the branch with support for Steam login so I can get back to that soon. Board checkpointing took a little longer than planned but is working nicely now. At the start of each play session a new file for the board is created on the backend so we keep a history of those boards. For now we will just let these pile up but we'll have to look into how many we want to keep around in future.

Another boring but necessary feature from today was adding limits to the number of campaign invite codes a person can make per minute and per day.

Tomorrow I want to look at having queries check with the connection manager to see if they should bother retrying on failure. I also need to look at the error codes from the backend queries so we can give meaning messages or take specific actions. After that it might be making the version of the steam login that we will use in the alpha.

Along with this we are squashing bugs but also finding more so the deadline is still looking very tight. I mean the worst thing that happens is the alpha is in January, but man.. I'd really love to get it out this year.

Ah well, gotta keep hammering.

Seeya tomorrow
