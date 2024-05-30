---
layout: post
title: TaleSpire - Bug Month Patch 7
description:
date: 2024-05-30 09:24:30
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again folks,

In this patch, we have a few tweaks and an experimental thing we'd like to hear your opinions on.

We have:

- Set 800x600 to be the smallest resolution pickable. This avoids a case where the window became so small you couldn't change the resolution back again.
- Fixed the stats in the radial menu so that they update immediately if another player changes them (before you'd have to close and re-open the radial menu.
- Added result counts to community mod browser folders

![/assets/videos/resultCounts0.gif](gif showing the result counts in the mod browser)

Now, on to the experimental thing. As we all know, using the community mod browser needs work. The search is somewhat lacking, and the experience of finding creations is sub-par.

A big part of the answer will be tagging. We are bringing tags to creatures soon™ (we'll have a dev-log on progress by Monday, and the tagging tool is going into internal testing on Tuesday), but in the meantime, I was wondering if there were any quick wins.

The search implementation is on mod.io's end so we can't tweak that. The current behaviour is such that if you search "Lord of the Rings" - it will return every result where the name contains any of the following words: 'The', 'Lord', 'of', 'the', 'Rings'.

There is another option, though. They have an option[0] where you can use `%` as a wildcard. That means you could search `fi%b%` and get results with both "fireball" and "Firebird."

So here is the question: Is that useful to any of you? It sounds like it could be, but in theory and in practice can be very different.

To that end, I've added it to the game so you can try it out. By default, the search is the same as before, and the wildcard symbol won't work. However, if you start the search with a question mark, it will switch to the mode where you can use wildcards. For example, `?fi%b%` matches "fireball" and "firebird" as mentioned above.

![/assets/images/wildSearch.png](Showing the result using the wildcard search)

That's it for today. We'll be back soon with more fixes.

Ciao

*BUILD-ID: 14543672 - Download Size: Win / Linux 4.9 MB / Mac OS 7.8 MB*


[0] Fellow nerds might like to check out their api docs. The two places I think are most relevant are https://docs.mod.io/restapiref/?http#get-mods and https://docs.mod.io/restapiref/?http#filtering
Do let us know if you find anything interesting!
