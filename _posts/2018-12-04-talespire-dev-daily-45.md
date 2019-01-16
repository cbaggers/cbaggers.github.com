---
layout: post
title: TaleSpire Dev Daily 45
description:
date: 2018-12-03 23:47:07
category:
tags: ['BouncyRock', 'TaleSpire']
---

Ah today has been fun again. A few things that were done were:

- A few more little fixes to serialization of unique characters,
- If you delete a character that is unique, remove it's 'uniqueness' server side
- Added text to help debug fog of war (this was great, more below)
- fix two bugs in fog of war search when the search crossed zones
- make sure that when the octree node is larger than a tile that the fog of war search takes that into account in calculating distance walked (just fixed up some lazyness when coding the feature)
- Fix case where creatures you don't control seem to load in an incorrect position when you join and existing play session on a board
- Fix dumb bug where non GMs weren't loading unique creatures properly if they were the first player to reach a board
- Fix bug in quadtree deserialization
- Serialize fog of war for unique creatures. Sync it to players on join and store it in board so you can leave and rejoin board much later and it is remembered.
- Add code to recognise and upgrade boards stored in an old format
- other misc small fixes

Of these the most fun was the adding some additional info to the debug view of the fog of war so I'll jsut rabbit on a bit about that. This really is one of the places where Unity shines, its so easy to add additional UI to the editor, not just in the inspectors but in the scene view too. I was struggling working out why the fog of war was behaving strangely so I decided to show a number at every 'step' of the search to show how far the character could still walk (how many steps remaining). It took a few tweaks as the fog of war search is done on a different thread and UI elements have to be drawn from the UI thread, also I wanted the text to remain visible for a number of seconds. Anyhoo the result was that every time you placed a creature the scene view looked like this:

![debug0](/assets/images/fillDebug.png)

*the red circle is where the creature is*

When I did this in the morning I could immediately see that the numbers were wrong when the search crossed a boundary. Turns out that was a simple case of not initializing an int in one of the constructors for the search. Also there was a case where the number went from 0 to over 4 billion, that's an easy one, unsigned integer wrap around when subtracting 1 from 0. Each is a simple issue but it just became obvious as soon as I could see a tiny bit more info. Yay tools!

Out of curiosity I added a little slider to the creature's inspector to control how far the search would walk, this was great to fine tune and find a default value that felt nice.

![debug1](/assets/images/fillDebug2.png)

The other stuff doesnt make for nice pictures but is all obviously needed for the alpha.

Here's to tomorrow, hopefully it's as productive.

Peace
