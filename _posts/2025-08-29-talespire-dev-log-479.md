---
layout: post
title: TaleSpire Dev Log 479
description:
date: 2025-08-29 18:53:17
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks.

Today I've continued working on tags. The big remaining task on my todo list is adding language support to the fuzzy name search. So, of course, I distracted myself working on search performance.

First off, a quick overview. In TaleSpire today, you can search for creatures using tags. In an upcoming version, you'll be able to search not only by tag but also by the creature's name. We also intend to make search results update live as you type, rather than having to hit return before it starts the search.

The code to do this is fairly straightforward. We take the text you typed in the search field and loop over all the creatures names computing a score which indicates roughly how different your text is from the name we are checking. If the score is low enough we include the creature in the result.

The score is a not based on real research, it's mostly a couple of [levenshtein distances](https://en.wikipedia.org/wiki/Levenshtein_distance) with some tweaks that got us good results back when I wrote the search last time. For now I'm sticking with this approach so I just wanted to make it faster.

I first wrote the search as a [Burst](https://docs.unity3d.com/Packages/com.unity.burst@1.8/manual/index.html) compiled job. Along with some common sense tweaks to avoid lots of allocations, the time to search for a tile (fuzzily comparing against the name of every tile) was ~3ms [0].

This seemed slow, so I turned to the Levenshtein distance function, where the meat of the processing is happening.

The implementation was very nieve, it filled out the entire table of scores before extracting the result (see the linked article above for pictures of these tables). A very simple optimization comes from looking at how the table is filled and realizing that you only ever need two rows in memory, the current one and the previous one. For a long asset name of 50 characters that means only needing 100 elements rather than 2500.

Next up, I decided to cap the portion of the name we would compare to 254 characters. This mean that the highest number ever stored in the table was 255, so we could use a byte. This means 1/4 of the entire row can fit in one cache line. It also means that we only need 512 bytes of data for the two rows.

Lastly, I looked at the fact that I computed two scores. One comparing the search term to the whole name[1], and one comparing the search term to a subset of the name. You might notice that this is duplicate work, so we extract the score for the substring partway through computing the score for the whole name.

These simple changes, which as usual amount to "not doing the dumb thing", make a big difference. The time for searching all the tiles went from 3ms, to 0.5ms, and searching all the creatures takes 0.05ms.

There is definitely more we can do, but this plenty good enough for a first version.

On Monday I'll get back to what I was meant to be doing and add the language support :P

Hope you have a great weekend,

Ciao.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*


[0] This is a very informal dev-log. We aren't aiming to be a robust education resource here, and so the timings are adhoc, and only from my specific pc. I'm including them mainly so we can get a feel for the speedup from the changes.

[1] Actually, I split the names into chunks at specific characters, but that doesn't affect the implementation of this bit.
