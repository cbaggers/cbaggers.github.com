---
layout: post
title: TaleSpire Dev Daily 3
description:
date: 2018-10-04 17:31:45
category:
tags: ['TaleSpire', 'BouncyRock']
---

Allo again,

Today wasn't the most satisfying. I wanted to prototype the undo/redo scheme I had doodled out but some of how floors was implemented was making it difficult. Even though we are redesigning the floor system this weekend it was more work to work around the issues than just tweak them so, after a couple of hours reaching that realization, I spent a good chunk of the day [yak shaving](https://en.wiktionary.org/wiki/yak_shaving).

With that somewhat out of the way I had a go at implementing the scheme. I already has working local undo/redo, so the task was making it work with multiple people editing simultaneously.

One example issue (which I mentioned yesterday) can be summarized like this:

> There are two GMs, `A` & `B` and each have their own undo/redo history
>
> - GM `A` places a floor-tile
> - GM `A` places a crate
> - GM `B` deletes the crate
> - GM `A` presses undo

One possibility is then when `B` deletes the crate, the history event for placing the crate in `A`s history could be removed. This way `A` can't attempt to undo or redo something that has gone.

I'm a bit adverse to deleting stuff from the history however as you lose data. Instead I wanted to try putting a flag on the history event that inhibits it. When `A` hits 'undo' it will skip over the inhibited event and do the one before it. The nice side effect is that if `B` was to undo the delete, we can uninhibit the history event in `A`s history again, allowing undo and redo to proceed as before.

The idea has edge cases though and, whilst I have seen a few of them, I'm not going to bog this post down with them yet. I'll do some more experimentation and then report back to you all :).

As for tomorrow I think I'm going to leave this on a branch and get back to a few other bugs that are more likely to be an issue when working this weekend. There is one rather nasty one regarding level loading an reuse of supposedly unique ids :)

Until then,

Peace
