---
layout: post
title: TaleSpire Dev Daily 30
description:
date: 2018-11-10 00:12:55
category:
tags: ['TaleSpire', 'Bouncyrock']
---

Evening all!

Today was spent testing the 'unique creature' code across multiple clients. This went well and found a good few bugs. I rewrote the board sync as that was on the todo list for a while and started looking at other aspects on sync which need to be better.

The first one on my mind is making sure that each client knows whether their version of the board is more recent than the version on the server, it's a simple problem but has a few subtleties to it. It's not good enough to know that the server version is different as it is possible that you are building a board on your own (for a future session) and the power goes out. We would like to use the cached version of the level on restart and know that it can be pushed over the server version. A simple version id can handle this but I'd still like to muse a little longer on the potential downsides of this.

Another thing I need to consider is how scriptable assets handled with regard to sync, we obviously want to be timely about pushing the state of things like locked doors & chests to the server but with modding it will be possible to create assets who change state near constantly, how frequently do we really want to sync this? I'm not gonna spend too much time of this now as modding wont be exposed in the alpha and next year I'll be adding support for huge boards which is going to require changes to sync anyway.

There are bunch of other little details but this will do for now,

Until tomorrow,

Peace
