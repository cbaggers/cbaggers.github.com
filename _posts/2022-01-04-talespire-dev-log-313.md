---
layout: post
title: TaleSpire Dev Log 313
description:
date: 2022-01-04 20:44:40
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks,

To support HeroForge and creature modding, we had to start supporting the spawning of creatures before their meta-data is available locally.

This means (amongst other things) that we don't know the head position and thus can't correctly calculate vision when a creature moves.

As getting this right is one of my blockers, I decided I would take today very easy and not force a solution. I wanted to play around and give myself time away from the screen to think about it.

Initially, I thought that I might just sync the head position to other clients when a creature is spawned and when a board is synced. However, this fails in the following scenario:

- Player A joins a board
- The board contains a creature they don't have locally
- Player A's client starts downloading the creature
- Player B joins same board before Player A's download completes
- Player A is now responsible for syncing the board to Player B, but doesn't have the data for the creature

Perfectly reasonable, but messes up the approach. After thinking it over and discounting another idea or two, I decided to sync the head position along with position and rotation when a creature is dropped (dropped in this case, meaning released from the player's "hand").[0]

This had me revisiting the code that handles syncing the creature drop, which was a little dated. I cleaned this up, and it should now be trivial to send this data. I'll add that tomorrow and start testing.

So that's it, not much for a day, but I'm glad I wasn't rushing this.

Have a good one.



*[0] The idea being that the player should have a creature loaded before being able to move it. I'll have to add some UX to communicate this clearly.*
