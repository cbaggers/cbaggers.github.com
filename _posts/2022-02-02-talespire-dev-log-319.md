---
layout: post
title: TaleSpire Dev Log 319
description:
date: 2022-02-02 00:53:38
category:
tags: ['Bouncyrock', 'TaleSpire']
---

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

Hi folks!

I'm back from my break and have been getting stuck into things for the last two days.

- I've updated the HeroForge convertor to use some metadata embedded in the model (previously, this was in a separate file)
- Thanks to some changes by HeroForge, I can now separate the 'extras' from the base so we can use our own base (which we need for the indicators)
- I've tested our code with the production version of their API (I have been using their api sandbox until recently)
- I tried making some very large minis to explore edge cases of the conversion process.
- I've merged the latest work from the master branch into the HeroForge branch.
- Worked on some server stability improvements (still got some code-gen things to finish there)

Tomorrow I'll be focusing on making tools to help the team with support tickets. Currently, things like restoring deleted boards rely on some hacky scripts I wrote ages back and are not in a state useful for anyone else.

I'll be visiting Ree from Friday to Monday to work on HeroForge integration together. I'm hoping to deal with most of my loose ends on the tech side during that time. It's optimistic, but we'll see how it goes.

I had a lovely break and am glad to be back into the swing of things again.

I should have another dev log for you tomorrow.

Peace.

![jotunheimen](/assets/images/jotunheimen.png)
