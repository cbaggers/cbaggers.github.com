---
layout: post
title: TaleSpire Dev Log 80
description:
date: 2019-03-12 16:42:54
category:
tags: ['BouncyRock', 'TaleSpire']
---

Welcome back,

Today I started looking at the tag filter for the asset panel. It's one of those cases where the current way the UI is set up is not great but it's better for us to work around it and get the feature out than it is to delay the feature for the rewrite. I've been poking around with the different components to see what the easiest way to get this in will be.

It's not the most exciting thing but here are some gifs anyway:

This one is from early in the day just checking that we can give tag suggestions. At this point the potential tags were hardcoded for testing.

![hardcoded tag search](\assets\videos\tagSuggest0.gif)

Here was from a little bit later testing that we could use tags from the asset database. At this point the only two assets to have tags were the two doors and the only tag was 'door' which is why you don't see other suggestions.

![filtering](\assets\videos\tagSuggest1.gif)

The next things on the todo list are:

- Add some UI to TaleWeaver (our asset tool) to allow adding tags
- Go through all our current assets adding tags
- Make the blue tag buttons in the search bar work.
- Get the code cleaned up to the point that it will be trivial to replace when we refactor the UI code.

When those are done this will be in a good enough to get into the alpha. I expect that will be on the weekend update.

Peace.
