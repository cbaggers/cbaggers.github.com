---
layout: post
title: TaleSpire Dev Log 414
description:
date: 2023-09-26 23:19:20
category:
tags: ['Bouncyrock', 'TaleSpire']
---

With yesterday's realization that some of Unity's handling of overlapping keybindings is broken, I needed to add our own workaround.

I knew I didn't want to try and build a complete solution, so the first version was just to handle the issue that caused undo/redo on Mac to be broken.

To recap on Mac, Undo is `Command+z`, and Redo is `Command+Shift+z`. This is an overlap; in order to hit the keys for Redo on Mac, you also need to hit the keys for Undo.

I've added a simple handler that detects multiple actions happening on the same frame and picks the one with the longest chain of modifiers. This works for the Mac undo/redo issue as both actions will fire on the same frame as `z` is the last key pressed and is the one that causes both to fire.

We will need to handle more cases in the future, but that's a problem for another day.

I've got some new builds progressing through the build pipeline, so tomorrow, I should be able to ship a fix for that. Then, I can get back to business.

Hope you folks are well,

Seeya in the next log.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
