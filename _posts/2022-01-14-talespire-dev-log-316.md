---
layout: post
title: TaleSpire Dev Log 316
description:
date: 2022-01-14 00:21:34
category:
tags: ['Bouncyrock', 'TaleSpire']
---

I've been bamboozled, hoodwinked, and led astray.

I was tricked into adding a feature I was not ready to add, but we will be shipping anyway.

Today we are talking about creature copy/paste!

So, creature copy/paste is a bit of a minefield. The reason it has been delayed is that supporting everything we want is hard. For example, if you can copy/paste a creature, you expect it to work with slabs, which means selection bounds should work with them, which really implies multi-delete should work too... and undo/redo.

This is not trivial. Multiplayer undo/redo gets hard in ways that you cannot get around with quick fixes. Explaining why would turn this post into an essay, so I'm going to ask for your forgiveness in not covering this and instead ask you to trust me on that point. [0]

I did try to get it working before the Steam release, but it got nasty, and it was a case of pushing back the release or delaying the feature. I'll admit that I didn't expect I wouldn't have revisited it yet, but c'est la vie.

Just a warning, this story doesn't end with all of the above being supported, but you will be able to copy and paste creatures in (and out of) the game soon.

This all started with me trying to add "creature blueprints." The idea of creature blueprints is to let you right-click a creature and generate a talespire:// URL containing all the setup info for that creature. This would allow you to share these blueprints with others. It was a simple, neat idea.

So, in short order, I had it working. You could right-click and click a button to get the URL. For convenience, we allow people to paste talespire:// URLs into TaleSpire to get the same effect as following the link [1]. It seemed like the correct behavior, so I added that too.

![first prototype](/assets/videos/first-prototype.gif)

I showed it to some folks, and they said it would be neat if Ctrl-C did the same as the button. It sounded good to me, and, dumb as I am, I didn't notice until I tested it that I had just been prodded into adding creature copy/paste [2].

![bamboozled](/assets/videos/bamboozled.gif)

Of course, it lacked all the complimentary features that I listed above. But this time I looked at it, I felt a bit differently. No, it does not work with undo/redo yet, but neither does spawning creatures right now. This means it isn't out of place in the current build. It also is only about copying single creatures at a time, so none of the expectations around selections apply yet.

Honestly, I should have realized this a lot sooner. I just have to chalk it up to experience and remember to challenge my assumptions more. "Perfect is the enemy of good" seems relevant here.

The next thing to crop up was that when you paste a creature, it comes with all its stat values. However, the names of the stats in the campaign the creature was copied from might not match yours. In the future, the stat names should probably belong to a rule system, and the blueprint should hold an id to the rule system in use. However, I don't want to repeat the same mistake by waiting for future features. So let's get something simple out.

![stat names](/assets/videos/stat-names.gif)

We are adding a copy button to the stats settings. Clicking it adds a URL to the clipboard, which contains the names. You can then ship these around to allow others to quickly apply the same stat names as you.

While not being the smoothest journey, it is working, and I want to get this out to you asap. We are still testing and ironing out some of the uglier bugs, but we haven't seen any scary release blockers so far.

More news on this coming soon,

Peace.



[0] Even just selecting and deleting is scary. What if you select 100 unique creatures, delete them, and then hammer undo/redo a little? The server has to be informed of all these changes, and that is no small amount of data. And yes, it's trivial to come up with ideas for handling this, but being good enough 90% of the time is not good enough. In short, this feature needs to be written slowly, carefully, and with testing.

[1] We added this when we saw confusion around how to use talespire://published-board URLs. Probably due to being used to how slabs worked.

[2] It really is staggering how blind one can be when being single-minded. I *knew* what I was making so clearly that what I was actually making was a surprise :|
