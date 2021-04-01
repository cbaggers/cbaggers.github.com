---
layout: post
title: TaleSpire Dev Log 264
description:
date: 2021-04-01 11:57:00
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Phew! Well, that was an 'exciting' start. 

The big upgrader bug was a use-after-release of some malloc'd memory (seriously, who allowed me to have pointers). Took a little while to track it down as different code would trigger the final crash. After that, it was about 3am so I slept like a log. 

Back today to start looking at all sorts of stuff. I need to take a quick look at a LoS issue while I'm with Ree, and collaboration is easier, but then I'll be back on a raft of bugs (especially upgrader-related ones).

If you've had issues with the upgrader, don't worry, the original board data is safe. I've written some routines that should also let us roll back and retry the upgrade if needed. We'll see how it plays out.

It's gonna be a heavy couple of weeks as we need to land more stuff for the EA release. Nothing to do but do it!

Special thanks to the bug reporters and folks digging through all this.

Seeya around the Discord :)

p.s. The new format for the slabs is coming. While encoding them in the slab and pasting them was simple, undo/redo brought with it a world of pain. Maybe something to chat about on a stream sometime.
