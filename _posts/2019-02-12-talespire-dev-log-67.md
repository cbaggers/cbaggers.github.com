---
layout: post
title: TaleSpire Dev Log 67
description:
date: 2019-02-12 23:21:28
category:
tags: ['BouncyRock', 'TaleSpire']
---

Evening all.

Yesterday we pushed out a little update. It was significant for us as it's the first update after upgrading the version of Unity we were using. That's always tense as it can be hard to ensure that everything is still working the way it was before. The update also included a proper fix for the bug from [log 61](http://techsnuffle.com/2019/01/29/talespire-dev-log-61) along with a few new nice to haves like shortcuts for camera rotation etc.

That bug fix was going to go out last friday but when we tested the release we saw that synchronization was broken between the clients for this like doors, chests etc. That was super weird and after a LOT of head scratching we found out that it was due to not having reprocessed a bunch of assets after the Unity upgrade. That was a bummer that we lost a day or two to it but good as rereading all the code helped me find and fix another unrelated issue.

Today I've mostly been looking at tickets reported by the community. We've not done a great job getting back to the awesome people who have filed tickets and that bums me out. Even though most get seen as they arrive the very least we should be doing is letting you know whether we can reproduce the issue. I'm gonna try to make sure I leave some time every day for that so I don't fall behind again.

Once again thankyou all so much for the super thorough reports that have been coming in, they truly are invaluable.

Now that the last big tickets are dealt with I have a small gap before the UI for the new features is ready so I will get back on tickets again. Expect movement on those real soon!

Time for some sleep.

Peace
