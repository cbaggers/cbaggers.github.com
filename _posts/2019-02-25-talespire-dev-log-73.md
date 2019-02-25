---
layout: post
title: TaleSpire Dev Log 73
description:
date: 2019-02-25 18:10:31
category:
tags: ['BouncyRock', 'TaleSpire']
---

Allo again, back with more of today's messing around.

In the morning as planned I fixed the case where the [Attack prompt remains if figurine is removed before confirming request ](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/68). During the testing I found that there was a case where you could get the following.

- Gm0, Gm1 and Player0 join
- Gm0 adds Creature0 & Creature1
- Player0 tries to attack Creature1 with Creature0 and the UI appears to indicate that the attack is waiting on approval
- On Gm0 & Gm1's clients the UI appears to approve of decline the attack
- Gm0 approves the attack
- The UI disappears on Gm0 & Player0's clients
- The UI is erroneously still visible on GM1's client

This was relatively easy to fix but testing took a while as after each attempt the build has to be copied to a couple of machines to test all 3 connections.

After that I added some platform specific code for controlling the hardware cursor so we can start tackling [this issue](https://github.com/Bouncyrock/TaleSpire-Alpha-Public-Issue-Tracker/issues/53).

Okidokey that's all for today. Tomorrow I start with a bug where deleting a creature can cause subtle issues with the turn based line-up ui and.. well then I'll grab another from the pile.

Peace
