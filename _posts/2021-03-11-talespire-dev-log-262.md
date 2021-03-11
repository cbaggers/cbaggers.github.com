---
layout: post
title: TaleSpire Dev Log 262
description:
date: 2021-03-11 04:12:46
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again, time to recap what I've been up to today.

Aside from fixing a bug in picking, my focus has been on board sync.

We will soon be syncing sections of the board on demand rather than downloading the whole board. This means a bunch of communication between the client and server requesting information about the board's state.

Naturally, instead of sending long lists of sections in the requests, we would prefer a more compact approach. To that end, we use masks a lot. Our regions of the board form a hierarchy:

- a board contains sub-boards
- sub-boards contain sectors
- sectors contain sub-sectors
- sub-sectors contain zones

Using sub-sectors is optional, and so I'm going to skip over those today. All the requests I'm interested in are about sectors.

Each sub-board contains 64 sectors, so a 64bit mask paired with a sub-board id is a very convenient representation for identifying a bunch of sectors. I had written the code to produce and manipulate these masks, but I had seen an issue a while back that suggested that the math was incorrect. Because of that, I put today aside for testing.

It's rather tricky to stare at a 64bit number and correctly picture the 3D mask it represents, so I knocked up a little project for visualizing them (Unity makes this very easy)

First a visualization for a single sub-board:

![single sub-board](/assets/videos/subBoardMask0.gif)

Each spherical indicator represents one sector. One the center is red, the bit is set in the mask.

From there, we build up to multiple sub-boards so we can test crossing boundaries:

![two sub-boards](/assets/videos/subBoardMask1.gif)

And then finally to testing a full board's worth.

![full board](/assets/videos/subBoardMask2.gif)

We can now be much more sure about the masks being produced. This is a relief as beyond server requests, they are also used to track modified board regions and whether they have loaded.

Tomorrow I'll probably be looking set separating the storage of script & non-unique creature state from the rest of the board data. This will minimize the amount of data changed when people are playing boards shared by others. Once the non-unique creature state has moved, I can at long last add copy/paste of creatures. I have held off on that for AAAAAGES as it makes it far too easy to add hundreds of creatures in a single action, and previously all creatures lived in the database, meaning lots of server requests.

Once creatures are added to copy/paste, I'll publish the new format so the community can be ready to support it when it lands.

Anyhoo I'll keep ya posted with all that in the coming logs.

Goodnight.
