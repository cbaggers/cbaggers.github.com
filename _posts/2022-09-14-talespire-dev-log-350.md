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

With Ree back from vacation, he has been let loose on making AOEs. He's made a more low-key variant of the ruler visual for the AOEs that is tintable. So yes, we will have color options for the AOEs on launch (this is landing in Beta in an hour or so). He's made it so that, as well as the keybinding, you can right-click on a ruler and click a menu option to turn it into an AOE marker. He's also made it so that the distance/angle indicators only appear when hovering the cursor over the AOE. This keeps the visual noise to a minimum

You can see a little of it in action here (although the visuals are not finished yet)

<iframe class="video" src="https://www.youtube.com/embed/4cSBeUWueWU?feature=oembed" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0"></iframe>

The latest round of playtesting has revealed a couple of things we don't like:

- The pixel picking for the AOE handles works great, but it's too easy to get handles stuck inside tiles. So we are looking at augmenting the AOE picking with something that feels more forgiving. I'm currently prototyping this and will be continuing tomorrow.

- For rulers, it feels right to see the handles through walls. For AOEs, it rarely does. We need to change that behavior before release.


## Group movement

As for me, I spent most of the last week staring at squiggly lines diagrams like these:

![squiggles](/assets/images/squiggles.png)

My goal has been working on a good way to divide them into simple polygons (see [my last dev log](https://bouncyrock.com/news/articles/talespire-dev-log-349) for the overview of what we need this for).

I have an approach that is quick and works in many cases…

![lasso working](/assets/videos/lasso1.gif)

…but not all of them

![lasso with hole](/assets/videos/lasso2.gif)

The above gif shows two holes between the two polygons. Now, this isn't uncommon. You are probably very familiar with what happens in graphics applications when you use lasso-select and self-intersect a bunch.

![lasso tool in gimp](/assets/videos/lassoGimp0.gif)

This behavior sucks for TaleSpire though. Actions like this should have 100% predictable behavior and trying to anticipate self-intersection behavior should be unnecessary.

This means I need to have another go at the polygon decomposition. However, what we have is plenty good enough for continuing with our experiments, so I'm going to leave it for later.

The next step is to write the ear clipping code that creates the mesh from the simple polygon. I'm back working on AOEs for now, but I think I'll be done there soon.

## Wrapping up

Well, that is probably the best place to leave it for now.

Hope you are having a great week. Hope to see you in the next dev-log.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
