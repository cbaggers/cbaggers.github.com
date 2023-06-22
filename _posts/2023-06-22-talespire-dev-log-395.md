---
layout: post
title: TaleSpire Dev Log 395
description:
date: 2023-06-22 08:05:40
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Currently, I'm looking into whether we can have mod.io support ready in time for Symbiotes leaving beta.

I know we won't have the in-game upload done for that time. But what if we allow uploads on the website and downloads in-game? That still should be a much better experience for regular players.

![in-game symbiotes browser](/assets/images/symbBrowser0.png)

It took minimal effort to wire up the community-mod-browser to show the symbiotes, so now I need to handle the subscriptions to mods themselves.

mod.io does have its own mechanism for subscribing to mods, but we would prefer to handle that for a couple of reasons:

1. It makes the code that handles mod.io more sharable with community-run repositories
2. We can let you decide who knows what you subscribe to.

Number two feels important. With that TaleSpire can give you the option of whether to only store your mod list locally or on our servers so that it is automatically available wherever you are playing. If we can provide that choice, we would prefer to.

Right, that's all from me for today. This week is pretty hectic as I have a bunch of guests over for midsummer, and I need to make sure I don't just hide away coding :p

Have a great one folks. More coming soon!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
