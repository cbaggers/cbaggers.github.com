---
layout: post
title: TaleSpire Dev Log 263
description:
date: 2021-03-13 03:17:06
category:
tags: ['Bouncyrock', 'TaleSpire']
---

I wanted to write an update, but it's all particular, so sorry this one isn't exciting.

I'm working on persistence again. A while back I decided that, to start with, we will have two files per sector. One which everything will be persisted into and one that will only hold some kind of changes. The reasoning for this is that when a person copies a board to play, they will be moving around creatures and using objects (chests/doors/etc), and we don't want that to invalidate the whole sector and cause a copy of the tile/prop data on the backend. So instead, the first file acts as a base, and the second optional file is applied on top.

I've updated the backend to support this.

I also set things up to keep the three most recent saves for each sector on the server. This will allow for a rollback in case of bugs. However, I'm still not sure what form the UI should take for this in TaleSpire itself.

I'm now focused on updating code to keep track of the board's modified regions (using the masks from yesterday's post) and changing how the non-unique creatures are persisted.

Tomorrow will be lots more of this, so I'm gonna go get some sleep.

Seeya around!
