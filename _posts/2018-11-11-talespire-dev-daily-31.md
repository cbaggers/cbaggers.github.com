---
layout: post
title: TaleSpire Dev Daily 31
description:
date: 2018-11-11 23:30:48
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today was spent chasing down smaller issues and, for features that need UI work, writing down behaviors which I don't think we have defined well enough.

As an example for the latter we haven't defined whether a player who does not have permission to GM can join a game without a GM. It might make sense as maybe the GM has said they'll be there in 5 mins. It could be cool to let the player look around and re-familiarize themselves with the board, however there are also certain tasks that a player needs to have certain permissions to perform and that player may not have them. To be more specific some server calls require someone with GM rights.

The first task will be to define what behavior we think feels best and doesnt pose any risk to the integrity of the play session. Then we need to work out what technically needs doing and whether it will be feasible (along with everything else) to get done for the alpha.

Please don't get to hung up on the example above though, it's just and example and there are plenty more tickets to review :)

Today I also worked a little more on sync, making sure host migration worked properly. There are still some details to take care of there but it requires some of that planning mentioned above. I also am finally writing the undo/redo code for multiple simultaneous GMs that will be in the alpha. This is resulting in some changes to the serialization code which should help us down the line.

As the alpha rolls onward we will need to be able to make changes to the file format that the boards are stored in. To support this I've started stubbing out the format upgrade code. This will just handle the case where a newer version of the game is opening an older version of a board file. Simple but necessary stuff.

All in all it's been an ok day. I'll kick of tomorrow by finishing the undo/redo stuff and then dive into the next pile of tickets.

Until then,

Peace
