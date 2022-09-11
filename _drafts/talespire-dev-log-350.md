---
layout: post
title: TaleSpire Dev Log 350
description:
date: 2022-09-11 18:53:22
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks!

It's been a good week for feature work, so let's get into it.

## AOEs

With Ree back from vacation he has been let loose on making AOEs look and feel good. He's made a more low-key variant of the ruler visual for the AOEs that is tintable. So yes we will have color options for the AOEs on launch (this has not landed in the Beta yet). He's made it so that, as well as the keybinding, you can right click on a ruler and click a menu option to turn it into an AOE marker. He's also made it so that the distance/angle indicators only appear when hovering the cursor over the AOE. This keeps the visual noise to a minimum

You can see a little of it in action here (although the visuals are not finished yet)

<iframe class="video" src="https://www.youtube.com/embed/2C49FD3e0_w?feature=oembed" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0"></iframe>

The latest round on playtesting has revelaed a couple of issues we dont like:

- The pixel picking for the AOE handles works great, but it's too easy to get handles stuck inside tiles. So we are looking at replacing the AOE picking with something that feels more forgiving[0]. We had a solid idea for this and I'll be prototyping it tomorrow.

- For rulers it feels right to be able to see the handles through walls, for AOEs it does not. We need to change that before release.


## Group movement

As for me I've spent most of the week staring at squiggly lines diagrams like these:

![]()

My goal has been working on a good way to divide them into simple polygons (see [my last dev log](https://bouncyrock.com/news/articles/talespire-dev-log-349) for the overview of what we need this for).

I have an approach that is quick and works in a lot of cases…

![]()

… but not all of them

![]()

The above gif shows two holes between the two polygons. Now this isnt uncommon. You are probably very familar with what happens in graphics applications when you use lasso-select and self intersect a bunch.

![]()

This behaviour sucks for TaleSpire though. Actions like this should have 100% predictable behavior, and trying to anticipate self intersection behaviour is unnecessary. 

This means I need to have another go at the polygon decomposition. However, what we have is plenty good enough for continuing with our experiments so I'm going to leave it for later.

The next step is to write the ear clipping code that creates the mesh from the simply polygon. I'm working on AOEs on Monday so I expect I'll be doing this on Tuesday.

## Wrapping up

Well that is probably the best place to leave it for now.

Hope you had a great weekend. Hope to see you in the next dev-log.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] It certainly gives me a giggle that I worked for a couple of days adding a new way for systems to integrate with the pixel picked, only for it to feel bad :P But that's just dev. The extension to pixel-picking will be useful in other cases for sure.
