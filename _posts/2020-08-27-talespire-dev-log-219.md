---
layout: post
title: TaleSpire Dev Log 219
description:
date: 2020-08-27 02:06:31
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today has been spent working on the behavior of uncommitted action.

Whenever you add or delete tiles, we need to perform the action on the other players' boards. To get the right result, the actions need to be applied in the correct order. This is done by sending all of these actions to a player, which was automatically designated as the 'host'. They then relay the action to everyone, which gives us an order. We call an action which has been given an order a 'committed action'.

Given that we can only apply the actions once they have been committed, we are in this annoying position where we need to wait for that roundtrip before we see a change from our keyboard/mouse click. That feels bad[0], so we needed to do something about it. By the time of the beta, we had made a system that could update the board's visuals for our uncommitted changes so that the change looked immediate. It was complicated, but it was made much more so by the fact that we had to spread some changes over many frames due to Unity not being able to keep up with spawning without impacting frame-rate.

Now that we have taken control of batching, we can happily spawn thousands of tiles a frame with no issues, and this lets us drop the performance workarounds. I'm now rewriting the code that handles applying visual changes for uncommitted actions.

It's going pretty well. I have uncommitted actions working for adding tiles and undo/redo of the same. I have made good progress on delete and, although it's significantly more complicated, I'm pretty confident I'll have it working tomorrow. If I can get this done in two days, that'll be pretty great as the last version took months to get right. [1]

As soon as this is behaving, I will be switching back to physics. The first task is to make the ground a physics object again, and then I will be looking at dice. I hope to have some familiar dice rolling gifs by Monday :)

Hope this finds you well folks.

Peace


[0] It might seem like not a big deal, but we had complaints about the responsiveness of undo/redo in the alpha, and back then, that delay was mostly due to performing the action on key-up rather than key-down. Input latency really matters.

[1] Of course, it's not an apples-to-apples comparison as the codebase is already structured in a way where this would be possible. However, it definitely feels much simpler.
