---
layout: post
title: TaleSpire Dev Log 258
description:
date: 2021-02-01 02:59:01
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks!

Small update from me tonight. 

After much wrestling with SQL quoting rules, I've got the partitions for board-data set up in the DB. I can now reserve an entry for a file, commit that once an upload has completed, insert the sector info into the correct place, and then query the board hierarchy to find out what files I need to pull for a given area of the board. To be clear, we aren't uploading real per-sector data yet, but the database doesn't know that :P.

With that looking promising, I started thinking about the upload/download managers again. To support resuming incomplete downloads, I knew I'd be using the HttpWebRequest API. However, I don't have that much experience with it so, as an exercise, I rewrote the upload/download routines that we currently use when syncing the entire board. This was great as I didn't get distracted overthinking the API or wondering how to get a realistic data-set. I could just use TaleSpire as it stands on our branch.

This went well, and now I feel ready to sink my teeth[0] into the real work of the managers.

One data-set I haven't talked about much recently is the non-unique creatures. We want to remove their data from the database to support more of them, which means they need to be stored somewhere else. I'll work out how I want this for the Early Access release, and then I can get coding.

Goodnight folks.


[0] No, not *sync my teeth*, and how dare you pun in this place.
