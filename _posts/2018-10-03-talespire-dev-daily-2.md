---
layout: post
title: TaleSpire Dev Daily 2
description:
date: 2018-10-03 17:04:39
category:
tags: ['lisp', 'cepl']
---

Today I refactored how we spawn and sync multiple tiles of the same kind which  is used when dragging out tiles when in build mode. The new implementation uses less memory to synchronize but more importantly has a single entry in the undo/redo history so you can undo a slab of tiles in one go. In the process of doing this I also simplified one way of sending messages which means there is less boilerplate code to write (less code less bugs).

The last third of the day was mostly spent doodling on the whiteboard to work out a nice way of handling how edits made by multi game masters can cause conflicts in the undo/redo history.

A simple is case is as follows.

There are two GMs, `A` & `B` and each have their own undo/redo history[0]

- GM `A` places a floor-tile
- GM `A` places a crate
- GM `B` deletes the crate
- GM `A` presses undo

What should happen?

If `B` hadn't delete the crate then `A`'s undo would have removed the crate. But the crate is already gone.

If we do nothing but move back in the history then `A` will be confused. A successful input without an output makes the user feel like either they did something wrong or the system is broken.

It feels like the sane thing is that if nothing can be done we skip to the previous entry. But then what do we do with redos?

Also if GM `A` dragged out 20 tiles and `B` deletes just one, what is the correct behavior. I think I'd expect the rest of the tiles to be removed. So then does redo restore them all?

I think I have a solution for this but I only got it partially finished today. I'll get back into this tomorrow and see how it feels.

If that doesn't feel nice then I am considering briefly showing a 'ghost' of the deleted object so you at least get the indication that *something* happened.

Right, time to get some food and get ready for the lisp stream in a few hours.

Ciao.

<iframe width="560" height="315" src="https://www.youtube.com/embed/uBbsZV9aTeo" frameborder="0"></iframe>

[0] Separate undo/redo is pretty important to avoid people undoing each other's work, this rapidly escalates to bloodshed.

-----------------------------

> Boring Caveats
>
> These are just my daily notes as I work on TaleSpire. It's so cool that people are interested in the game and it's way more fun to share this stuff that just keep it away, however nothing said here should be taken as any kind of promise that a thing will exist in a given release or work in the way stated. Stuff changes constantly and I'm wrong about at least 1000 things a day so whatever I'm stoked about today may well be tomorrows nightmare.
>
> So yeah, that's that. Back to the code :)
