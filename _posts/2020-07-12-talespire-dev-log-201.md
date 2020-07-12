---
layout: post
title: TaleSpire Dev Log 201
description:
date: 2020-07-12 21:07:32
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey again folks.

Saturday's work went well. I spent most of it reviewing the current code and double-checking the tile spawning implementation. We've put out a bunch of fixes since the beta release, and so I wanted to be sure the code still matched my design notes from earlier in the development. Luckily it did, and working through all the code paths gave me a much clearer idea of what needed doing first.

It looks like I can get something working without having to handle uncommitted changes[0] at the same time. This means the response time of build actions will be unacceptable (as it requires a round trip to the session host), but it will let me tackle the tooling and copy/paste before having to deal with that additional complexity.

That's all for now.

Seeya


[0] In TaleSpire, in order to keep network traffic low enough to be manageable, we send 'operations' over the network. These are instructions telling the game what to change on each person's copy of the board. For this to work, all operations must be applied in the correct order. However, games feel bad unless changes happen as soon as you request them, so we keep track of changes that have not been given their final order yet. These are what we call 'uncommitted' changes.
