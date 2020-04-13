---
layout: post
title: TaleSpire Dev Log 170
description:
date: 2020-04-13 18:48:03
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Allo, bug fixing was gone reasonably well today.

This particular issue is not resolved but is mitigated for now.

Since the launch, we have been seeing a very large number of boards fail to sync because the backend refused to acknowledge that the people upload were GMs. The reason the backend thought this was the case was that their session no longer existed in the database.

Each time you connect, you are given a session, and it's refreshed each time a keep-alive message is received from the game. The game sends keep-alive messages every 5 minutes. If a session is not updated for an hour, it is considered inactive and removed from the DB. It is also removed if TaleSpire signs out.

I guess we could sprinkle the word 'should' everywhere in the last paragraph as something clearly isn't doing what it's meant to.

The most obvious candidate was the code the removed the old sessions, and so I temporarily disabled it to observe the behavior. This didn't seem to have an effect, and so I needed to investigate signout as a potential source of issues. However, whilst this is happening, we are obviously losing people's boards, which is pretty heartbreaking, so we needed something sooner than waiting for data and hoping it illuminated the problem.

Each websocket connection made to the server spawns an Erlang process that is tied to that connection. When the connection dies, the process dies (and vice versa). We can store information along with this process, which allows us to tie information to your connection (the data remains server-side). We always authenticate you before creating the websocket connection, so the process almost represents the session we are interested in.

Ultimately I've wanted to move in this direction for a while but have not had time to. However, with things breaking, this afternoon became an exercise in how fast I can code carefully :D

It took about 6 hours to get the DB and server patches written and tested. We deployed at around 18:10 Norway time, and so far, it appears I'd only missed one thing. That was resolved quickly, though.

With that, all of the failures I've seen server-side about rights to save are gone. This does not mean all board persistence issues are fixed; it only means that this one cause is being handled. I still need to understand the session issue properly and keep an eye to see how things progress.

However, it does mean that this is no longer the highest priority. The next priority is now the 'HardAssert' failures that are corrupting board files. That is my task for tomorrow.

There is also likely to be another patch update either in a few hours or in the morning. We'll keep ya posted

That's the lot for now.

Peace
