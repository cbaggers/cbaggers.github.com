---
layout: post
title: TaleSpire Dev Log 333
description:
date: 2022-03-24 15:59:14
category:
tags: ['Bouncyrock', 'TaleSpire']
---

So while we wait to announce the release date, let's talk about some other details.

## The road to HeroForge release

The release itself is going to be a two-parter. First, there will be a short open beta, and then the more public "launch."

Now, this is a bit unusual for us, so why are we doing it?

In short, HeroForge has a much larger community than ours[0], and when they publicize a new feature, a lot of folks will rock up to try it out. By having a short beta before pushing announcements everywhere, we all collectively get to kick the tires and find any scary bugs that snuck in before any bigger rush of people.

There are no restrictions to participate in the beta beyond having a HeroForge account[1]. The doors will be open, and you are all welcome in, but we won't be doing a big PR push immediately, and the word "beta" will be slapped on a bunch of places.

Once both teams are happy, we will remove the "beta" label and then fire up the social medias, blast out announcements, and see how it goes!

The fact there is no restriction will confuse a few people, but after having you wait for so long, we couldn't stand the idea of only letting some of you in for the beta. This feels like a decent trade-off.

## Coming soon

Already we are turning our eyes to what we will be working on next. The art team will be busy with the cyber-punk and scifi pack, so it's definitely a good time to ship some features.

This is ideal as we have a ton of stuff that feels 80% done and just needs that work to get it out the door.

Even if we marked it experimental and required turning it on in the settings, it would be good to get this stuff out.

One example of this is the change to rulers I've been calling "tactical rulers" internally. That change means a ruler isn't cleared the instant you click on something else. This makes moving creatures (especially troops) pre-measured distances much easier. It is also a requirement for us to be able to start on area-of-effect markers (but that's a different story).

Another thing that marries well with tactical rulers is group movement. Ree has already put a lot of work into that, and getting the last 20% feels doable.[2]

I feel like I'm tempting fate if I start teasing the other features in that "80%" category, so I'll leave it for now, but I'm feeling very optimistic about what comes next.

## That's all for today

So yeah, that's the lot for this log.

More news in the next one.

Have a good day!



*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] But hey, no trades, our community rules!

[1] Of course, you or one of your party will need to have bought some digital minis to be able to bring them into your campaign.

[2] We will probably limit the number of uniques that can be added to a group initially. This is due to the amount of data that must be sent to the backend on each movement. I know what changes I need to make on the backend to make this more efficient, but that will take some more time.
Non-unique creatures are handled totally different and so won't have that restriction.
