---
layout: post
title: TaleSpire Dev Daily 23
description:
date: 2018-10-30 20:47:47
category:
tags: ['BouncyRock', 'TaleSpire']
---

Motivation is a fickle thing, after cranking through a bunch of quick tasks I end up craving something more meaty and meaningful to slam my head against, but give that month and I'm just craving the quick reward cycles of small tasks again.

Today was a switch back to the latter, I've been neck deep in the search stuff for so long the deadline went from feeling insane to hopeless. Getting that merged in late last night let me move onto refactoring how joining campaigns and boards is handled and it's felt great.

A lot of the work has been tracing down what code owns what and massaging it into a new shape. [Photon's PUN](https://www.photonengine.com/en-US/PUN) library (the networking lib we use) has a concept of a 'room' where every client in the room can communicate. This maps nicely to our boards and this is what will allow us to have a single campaign with different party members in different boards concurrently (each with their own GMs if you have the people for it).

I have also stubbed out the code for the local and server autosave and started looking into how we handle 'unique creatures'.

Internally we talk about any controllable thing being a creature, whether it's a humanoid hero or a sewer rat. Most creatures below to the board and are stored with it, however a creature can be made unique which now means their data lives on the campaign and they can be moved between boards. Every player's character is naturally a unique creature, but a GM can take any creature and make them unique. This can be cool for story reasons for example, maybe due to the player's actions the aforementioned rat touches a forbidden emerald and becomes imbued with terrible knowledge, purpose and dark magic, the GM may want to have that rat be a reoccurring part of the story so making it unique makes that much easier to manage.

Ah I cant wait for all that to be working, tomorrow will be finishing off this board refactor and then I'm getting that unique stuff added. That will require changing serialization of both boards and campaigns but it shouldn't be that tricky.

Until tomorrow,

Peace
