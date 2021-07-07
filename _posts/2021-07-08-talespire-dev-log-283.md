---
layout: post
title: TaleSpire Dev Log 283
description:
date: 2021-07-07 22:56:30
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again folks. I'm just dropping in to give an update on my last few days of work.

My current work is focused on changes to support upcoming improvements to creatures. These include polymorph, eight stats per creature, and persistent emotes.

Creatures are a pain to change as unique creatures are stored in the database, whereas non-uniques are stored in a binary format per board [0].

Beyond that, polymorph means we are storing up to ten asset-id and scales per creature. When creature copy/paste eventually lands, it will make it easier to unnecessarily bloat the non-unique data. Because of that, I'm now aggregating the ids into an array and storing indices into that array [1].

Given that we are having to make this new format, it gets very tempting to make other changes too. I spent some time playing around with some ideas here but decided against doing anything more dramatic for now.

Polymorph is one of those features that touches a lot of code. Up until now a creature doesn't change its visual once it has been spawned. This assumption is pretty baked into the creature code, so changing this is more fiddly than it might seem. Currently, I'm writing the server code for adding/updating the new creature data, but after that, I'll have to bite the bullet and update that part of the client-side code.

If all goes well, I'll be visiting Ree next week to work on the game in person. It's been quite a while, so that's pretty exciting.

Have a good one!


[0] The uniques being in the database makes it easy to search across the entire campaign. The format for non-uniques is more efficient for bulk updates.
[1] As an example, let's say you took a creature with three morphs and used copy/paste to make an army of 50 of them. In that case, the naive approach would take 2400 bytes to store the asset-ids. The aggregate approach would use 148 bytes.
