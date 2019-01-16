---
layout: post
title: TaleSpire Dev Daily 52
description:
date: 2018-12-13 23:49:47
category:
tags: ['BouncyRock', 'TaleSpire']
---

Today has gone well. Not a whole bunch of stuff to tell but the invite panel is fine now and ui work is continuing steadily.

I found some bugs that require a tiny bit of context to explain.

Every tile in the board has an ID the game can use to address it. These IDs are how the network code specifies a tile when it wants to say something like 'delete that tile'. Naturally IDs need to keep the same on every client and no two tiles should have the same id.

The bugs resulted in duplicate IDs. What was happening was that we synchronized the IDs when sending the board and we weren't including the ones from things that had been deleted and were in the undo/redo history. This could result in the following sitation:

- Player 1 places 10 tiles (ids 0 - 10)
- Player 1 leaves
- Player 2 deletes some of the tiles (ids 5 - 10)
- Player 1 returns and Player 2 sends Player 1 the board (sending ids 0 - 4)
- Player 1 places 3 tiles (starting at 5 as it thinks that is free)
- Player 2 hits undo (restoring tiles 5-10)
- tiles 5-8 now have duplicates.

We could either issue new IDs on undo/redo or just make sure we keep better track of ids in the undo/redo history. The latter felt like less of a band-aid so I refactored the code to handle that. It took a while to test enough to feel comfortable that it was behaving properly again but it seems ok (comforting no?).

This delayed some things but is always worth it as it's such an ugly bug when it appears as it normal manifests differently on the different player's clients.

Tomorrow I'm back on session history which in theory should be simple. I think after that I'll need to tackle one of the remaining tickets which could require significant re-engineer in the worst case (most of the tickets have fairly understood scope).

Until next time!
