---
layout: post
title: TaleSpire Dev Log 163
description:
date: 2020-03-07 00:02:23
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi again folks!

Work is going well. For the last two days, I've been focusing on handling network failures and testing board sync.

Sometimes you just lose connection for a moment; in that case, we want to get you back into the game as fast as possible. It's quite likely that, since the client dropped out, nothing significant on the board has changed. In those cases, we can quickly confirm we are still up to date, make a couple of small tweaks and carry on as if nothing happened.

If there have been changes to the state of the board that aren't trivially to reconstruct, we simply reload the board, which will bring your client back to the state it should be.

Another thing that can happen when someone leaves is host migration. When you have multiple people playing in TaleSpire, one client is the 'host'. The host does the book-keeping that decides the order that all changes to the board happen in. When that client leaves that job moves to another client.

However, what if there were messages that were sent to the host before it dropped out. Did they make it? If they did, have they made it back to the clients yet? How do we know when to check? Then, if we are the sender of a lost message, we need to undo the change it made. Getting that nailed down was another of today's tasks. I have a first version in, but it could do with more testing.

I also found some smaller bugs in board sync, which are now fixed, and I squashed a few other little things on the way.

Next, I'm finishing off the changes to board-sync to make it handle failures more gracefully, and I also need to have a look at pasting of slabs saved as strings as I think that is misbehaving right now.

Until next time,
Ciao!

p.s. I really was tempted to go into more detail about these fixes, but doing so would require a bunch more context, and I'd like to work on a couple more things before calling it a night.
