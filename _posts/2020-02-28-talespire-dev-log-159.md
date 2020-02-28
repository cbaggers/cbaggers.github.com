---
layout: post
title: TaleSpire Dev Log 159
description:
date: 2020-02-28 00:32:33
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today work progressed pretty well. I've updated the code that handles tile deletion and have moved over to fixing up tests. Copy and paste still need to be updated, but I'm more likely to find fundamental issues in the code I already have, so I definitely want to get that fixed up first.

I'm fixing up small mistakes from the refactor, and once that is done (and all current tests pass), I am going to start adding tests to try and find bugs when multiple builder's operations interleave. Ideally, I want to use my automated testing code to find the bugs, but there are some limits here.

To perform automated tests, I made a very simplified implementation of the board, and it's core operations, we call this the model. I then take the model, an instance of the actual board class, and a stream of random board operations. I apply the stream of operations to both the model and the board and then compare the state of each to ensure we got the same results.

The model, as mentioned, is very simplified. One simplification is that it doesn't support multiple builders. This means that while it's great for testing against one board, it's not so useful for testing against two boards that are being manipulated by separate players.

We still can get some useful data, though. If we take a board and have two players add and delete things, it should be the same as if a single player performed those same actions in the same order. This means for those operations, we can use our model. However, undo/redo operate on a player's own action history and so can't be modeled the same way.

Regardless of the limitation, it's still useful to use the automated tester, so I'm definitely going to implement the add/delete case mentioned above.

I should have that working tomorrow if all goes well.

Seeya then!
