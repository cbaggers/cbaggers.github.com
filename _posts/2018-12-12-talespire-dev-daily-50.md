---
layout: post
title: TaleSpire Dev Daily 50
description:
date: 2018-12-12 00:29:24
category:
tags: ['BouncyRock', 'TaleSpire']
---

I hadn't expected to be doing a fairly hefty refactor of the board loading today but that's where I've found myself. I'm trying to nail down the behavior that occurs when the user you are syncing the board from leaves during that sync, or if the sync fails, or.. well you get the idea, we are looking at failure cases and how they behave.

The downside is having to retest a bunch of basic functionality but naturally the upside is that we are fixing issues the old one had.

The new UI hasn't landed yet so nothing new to report there yet.

The only other important thing I've been working on today is adding a retry callback to our http request so that, in event of failure, you can choose if the request is retried or not. This was a pretty quick change as a bunch of the csharp code that wraps the calls to the backend is generated from a description on the erlang side. This has made it easy to make frequent changes to the backend api and know that the frontend code matches it exactly. More on that another time though.

Goodnight folks. Tomorrow is just gonna be more of this robustness work.

p.s. Reentrancy in state machines is a pain
