---
layout: post
title: TaleSpire Dev Log 166
description:
date: 2020-03-20 20:00:26
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again,

Yesterday I managed to get 'unique creatures' working in TaleSpire again. A unique creature is a creature which has has been marked as special by the GM, it is no longer tied to a single board, and it's hp, stats, etc. are retained across the campaign.

We use [Photon](https://doc.photonengine.com/en-us/pun/current/getting-started/pun-intro) for the realtime networking in TaleSpire. Anyone who is connected to the same board is connected via the same Photon 'room'. When you are in the same room, you can send and receive RPCs, events, and synchronization messages.

However, if someone is in another room and summons a unique creature, we need to remove it from any board it was already in. We can't use Photon for that, so instead, we use our backend. When you make tell the backend that a unique creature has entered a board, we broadcast a message using [erlbus](https://github.com/cabol/erlbus) and then all listeners send a message down to TaleSpire itself. Naturally, you wouldn't want every client receiving all messages, so we use topics. When you join a campaign, we subscribe your client to a topic named by the GUID for that campaign, and we do the same when you enter a board. This means it's trivial to send messages to any client regardless of if they are in the same Photon room or not.

All of that appears to be behaving itself (although it could definitely feel better), and so today, I've been working on a selection of client-side bugs.

- Torch state not syncing
- GMs couldn't focus the cutscene camera of players
- GMs couldn't remove specific dice groups (clear all worked though)

All of these are now behaving again. Next, I need to have a look at tile drop-in and some cases where things remain highlighted after the selection is removed.

I think tomorrow I'll look at line-of-sight again and hook up the bit that actually hides and reveals creatures based on whether the creature you have selected can see them. Also, I'm 99% sure I'm sampling the cubemap incorrectly when collecting Ids, so I need to look at that and see what I'm screwing up.

Seeya tomorrow!
