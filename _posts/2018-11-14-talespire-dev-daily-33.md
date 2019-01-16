---
layout: post
title: TaleSpire Dev Daily 33
description:
date: 2018-11-14 23:45:06
category:
tags: ['BouncyRock', 'TaleSpire']
---

Hi again folks,

I'm down at [@jonnyree's](https://twitter.com/jonnyree) place right now for a couple of days hacking on TaleSpire and it's been going great. We have been going through the features that will be in the alpha and making decisions of behaviors that, up to now, were a bit vague. That has unblocked a bunch of tickets so they are all on my todo list for the next couple of weeks (not that I have been short of work).

Currently I'm fixing some bugs by setting up some simple state machines for managing the current game mode and client state. This just makes some things more explicit and makes the transitions between the states very explicit.

Tomorrow I'll switch from this onto the rewrite of the placement code. This is the bit that handles the behavior of picking up and moving pieces. Naturally it's critical to nail the feeling here as it's its your primary interaction with the game. It seems like you could just attach the piece like a pendulum and let physics drive the experience but nope :) Lot's a pair programming will be on the cards.

Alright, that's the lot for tonight.

Peace
