---
layout: post
title: TaleSpire Dev Daily 24
description:
date: 2018-10-31 17:24:21
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks, pushing this boulder up hill is slow going but it's getting there.

Today I continued work on the 'joining campaigns and boards' code. Mainly I was looking at how we can used locally cached versions of the boards to make sure that when you log in you see stuff immediately. We of course ping the server to get the latest info on things like the campaigns you are a member of, but those requests can take a few seconds and the most common case is starting a new campaign or continuing and existing campaign.

Whilst working on the database side of this I was touching code I would have to revisit immediately when I got to 'unique creatures' (see yesterdays post for details on them) so I decided that I would write the tables and queries for those things today.

And that about brings us up to now, tomorrow will start with exposing this to the web front-end and then its back into the engine to consume this new stuff.

Seeya tomorrow!
