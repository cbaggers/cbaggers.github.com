---
layout: post
title: TaleSpire Dev Log 143
description:
date: 2020-01-09 23:58:13
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today I got a useful portion of the automated testing working. It generates lists of changes (add/delete/undo/redo) and applies them to:

1. An instance of the actual board data-structure
2. An instance of a simplified model

There is also a second instance of the actual board data-structure connected to the first via a dummy network interconnect. This means that once we have applied a randomly generated list of changes, we can compare all 3 to see that all the results line up.

This isn't a substitute for other tests as all could be wrong in the same way, but it's already allowed me to improve undo-redo and track down a few small bugs.

I don't have any automated test case minimization yet, but so far, it's been relatively easy to minimize the cases that came up.

Next, I need to look at how data is moved to the 'inactive set'.

Each client has an undo/redo history with a fixed length (currently 50). When the history gets longer than 50 the history event is no longer reachable. This means it cannot be undone. It also means that we don't necessarily need to store it in the same way as we do for 'active' asset tile & prop data. Having a different data set, with a potentially different layout, can let us have optimizations that only make sense on that set.

To be moved to the inactive set, we need to know that the data isn't going to be modified by an undo/redo still in the active portion of the history. This evening I've been implementing the code to handle this. It's not done yet, but I'm hoping to wrap that up tomorrow morning. I'll then start testing longer streams of operations to make sure they behave correctly.

I'll then be back on the bug hunt. Before Christmas, I saw a case where undo/redo started getting me some very odd results (big chunks of tiles missing). I'm not sure if the bug was in the data itself or the code handling the progressive spawning/deletion of the game objects. I'm still hoping the former as that's way easier to test, but I'm not sure quite how the bugs I've fixed the last two days would account for that. Ah well, we'll see soon enough.

Thanks all for tonight,

Ciao
