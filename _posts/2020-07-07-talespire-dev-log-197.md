---
layout: post
title: TaleSpire Dev Log 197
description:
date: 2020-07-07 02:43:56
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks, I hope your week has started well.

Ree has been busy taking care of business and working on rulers, and I've turned my attention to the backend.

I really want to get TS streaming zones rather than downloading the whole board, but before I do that, I want to look into any issues in the current system.

First orders of business are finishing the changes to session management and removing a bunch of old code. We have had quite a few updates to the server code since launch, and the old code has a mental cost when working on the system. 

Most of this is relatively straightforward. However, it requires a lot of care and double-checking to make sure the updates never interrupt service.

I'll probably push the server update tomorrow with is the safest part (adding new code). I can then update TaleSpire to use these new entry-points, and then soon we'll be in a good position to remove the old ones.

Until next time.

Peace.
