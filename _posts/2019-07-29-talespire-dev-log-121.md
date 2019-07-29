---
layout: post
title: TaleSpire Dev Log 121
description:
date: 2019-07-29 15:31:15
category:
tags: ['Bouncyrock', 'TaleSpire']
---

The end of last week looked at lot like this:

![undo/redo lisp test](/assets/images/undo-redo.png)

This was because I want sketching out ideas for the undo/redo system and I wanted a nice simple model for doing this. This is 4x4 map where tiles are represented by letters.

These tests allowed me to find small mistakes in my assumptions and to fix those in a much simpler (and less distracting) environment than doing the work in Unity. It was made it trivial to test out how I want to resolve the conflict between needed to apply changes immediately and needing to apply changes in the same order on all clients to get the right result.

I was using lisp for this as it's where I'm most comfortable and also because the ease of recompiling smaller chunks of code just makes for nice fast iteration times.

Since then I've been looking at fog of war but that has mainly been prep for the planning sessions that will take place when Ree is well. So nothing to show there is it mainly amounts to lots of pages of my notebook being covered in scribbles :)

For now, that's the update.

Keep an eye out for more news coming soon!
