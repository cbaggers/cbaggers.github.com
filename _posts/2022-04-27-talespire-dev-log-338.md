---
layout: post
title: TaleSpire Dev Log 338
description:
date: 2022-04-27 01:09:18
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hellooo!

We have passed the first 24 hours of having shipped the HeroForge integration, and things are going well!

I'll be honest that there were so many moving parts to this that I was expecting everything to be on fire at this point. It's nice to be wrong.

Yesterday's release included not only HeroForge support, but also:
- the vast majority of the code for the polymorph feature
- a jump to the latest version of Unity
- changes we needed to revisit creature copy/paste.

Today I've been working on bugs, a couple of which I'll highlight here:

One is a regression to some of the sign-in code, which has left a few people stuck on the main menu.

Thanks to a community member, we've been able to get some very helpful logs, and we are testing some possible fixes over the coming days.

Another issue was that minis with transparency were failing to convert. While we aren't supporting translucent HF minis, we don't want the conversion to simply fail. It turned out that the issue was that those minis were missing a texture that we otherwise expected to be there. I added a new code path to handle this case.

In the near future, HeroForge will be updating their site to give more information about what is supported. So hopefully, there will be much less chance of unwelcome surprises then.

That's probably the most interesting stuff I have to say for now. I'm diving back into feature work now! I think I will continue working on the next version of the rulers feature. Until next time.

Peace


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
