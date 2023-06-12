---
layout: post
title: TaleSpire Dev Log 390 - Symbiotes Documentation
description:
date: 2023-06-12 16:35:37
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Part of getting ready for the first Symbiotes release is finalizing the documentation. Or rather, I should "was" as that is now done!

We know that a bunch of you in the community are developers who might be interested in peeking at this even before the feature is available, so we have put the docs and examples public early.

You can find the documentation at [https://symbiote-docs.talespire.com/](https://symbiote-docs.talespire.com/) and the examples are at [https://github.com/Bouncyrock/symbiotes-examples](https://github.com/Bouncyrock/symbiotes-examples)

The documentation is also a valid Symbiote. That can be found here: [https://github.com/Bouncyrock/symbiotes-docs](https://github.com/Bouncyrock/symbiotes-docs)

We have intentionally not added calls that modify board or campaign state in this first API version. That allowed us to avoid the impact of such changes on the backend. However, we will be significantly expanding the API into those areas in the future.

Major props go to @Chairmander, who has been driving the entire documentation process. Left to my own devices, the first version would have been much more bare-bones!

You should also see a little patch going out in the next 12 hours. This will be the patch that adds UI that we will use during future maintenance periods.

Well, that's all for today!

Ciao


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
