---
layout: post
title: TaleSpire Dev Log 332
description:
date: 2022-03-23 10:13:35
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks!

Things are going great over here. Here's the latest from me.

## Server stuff

Last week was given almost entirely to server work, continuing the work from [the previous log](https://bouncyrock.com/news/articles/talespire-dev-log-331). I was able to get our [Packer](https://www.packer.io/) scripts up to the point where it completely replaces how we were building our old docker containers and Amazon AMIs. They're also way easier to maintain.

## HeroForge

We had a planning meeting with the HeroForge team last Friday, and we now have our internal timeline, which is super exciting for us. I say 'for us' because we aren't telling you the date yet so as not to put extra pressure on both our teams.

While that date is 'soon,' I've probably said 'soon' in so many different ways already that it doesn't really convey how soon we are talking, but you'll see pretty quickly from what I'm up to how close things are.

Speaking of which!

I am currently working through my last coding tasks marked **mandatory** for release. All of these are related to handling failure cases and how to expose the issues to the user.

Then I'm onto the tasks marked as things we really should have. This currently includes:

- Adding a panel for managing errors (which will be essential for modding)
- Changing where the HeroForge thumbnail data is stored

Ree has been working on the UI, and as soon as that lands, I'll merge my stuff and start recording videos of the entire flow of importing minis. It's a very short process, but we need tutorial content for TaleSpire and HeroForge's sites.

That takes us to the rest of the work leading up to release. We need to:

- Produce various texts, videos, and images for documentation
- Set up a new Steam branch for testers
- Chase things up with lawyers (for the wording of the ToS around HeroForge)
- Work out our point of contact for HF if they need support
- Write up our PR stuff for the release
- More testing

Writing stuff for humans always takes way longer than I think, so I'm aiming to have as much of the coding done by the middle of next week as possible.

Aaaaaaaa it's exciting!

We have also been preparing for the feature work we will be doing immediately after this ships. I think we'll be able to have some cool features landing before sci-fi ships :)

Alright, enough vague info for one day! I'll be back soon with as many extra details as I can share.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
