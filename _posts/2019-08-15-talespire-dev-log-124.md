---
layout: post
title: TaleSpire Dev Log 124
description:
date: 2019-08-15 23:40:30
category:
tags: ['Bouncyrock', 'TaleSpire']
---

It's been a nice day today, both @Ree and I got some coding done. Here are a few highlights:

- Dice feel regressed a while back and Ree got that fixed back up again.

- He also updated the implementation of the radial menus which we hope has fixed the issues with some stats not showing. I tested this but we had never been able to get a reliable reproduction of the issue so we will see how this fix holds up in the wild.

- I found a *interesting* default behavior in the networking library we used which was resulting in huge hangs when updating certain player information. Once found the fix was trivial.

- We have enabled Unity's incremental garbage collector to smooth over another GC spike we were seeing. It's very much a band-aid but it's a nice one for the alpha before we get to addressing the core issues behind the allocations.

More news and an update to the alpha in the near future.

Peace
