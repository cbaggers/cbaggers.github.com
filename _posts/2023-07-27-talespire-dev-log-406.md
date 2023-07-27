---
layout: post
title: TaleSpire Dev Log 406
description:
date: 2023-07-27 09:30:08
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks. 

I've narrowed the macOS issue down to something happening to the library we use for the Symbiotes web view.

The library itself seems to be innocent, however. My current assumption is that our build process accidentally does *something* to the library that stops it from loading. The good part, of course, is that we now know where the problem is, so all that is left is the "why."

That was a bit of a short update, so let's have a look at something unfinished Chairmander and I have been looking at between other jobs. Symbiote Login Issues.

The speedy explanation is: Lots of websites use other services to handle their login. That often requires some kind of popup where you log in to that service (e.g. google), and then you get redirected back to the site you cared about.

We didn't have support for that in Symbiotes, and so creators have hit a bit of a roadblock.

Luckily Vuplex (the web view library we use) has a hook for sites trying to open popups, so I hacked together a little test.

<iframe class="video" src="https://www.youtube.com/embed/LJPKTJ8LVwo" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0"></iframe>

The popup has some minor limitations compared to regular Symbiotes. But for login, this will probably do the job just fine. It still needs a bunch of testing and likely some tweaks, but it looks like even more useful Symbiotes will be coming soon!

I'm gonna grab a coffee and get back to it.

Thanks for stopping by.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
