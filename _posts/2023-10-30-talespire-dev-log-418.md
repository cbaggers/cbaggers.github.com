---
layout: post
title: TaleSpire Dev Log 418
description:
date: 2023-10-30 19:38:26
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks! I've not written in the last couple of weeks as we've been going hell for leather, trying to ship two patches a week.

That's been very successful, and it's a lot of fun to do from time to time, but it is pretty unsustainable when you hit bigger features. So this week, we are only aiming to ship the board folders. The good news, however, is that the feature I'm chipping away at now is creature mods.

So, where are we at?

Unity can pack stuff into things called AssetBundles. They are very flexible, but one downside is that you need to build one for each platform you are targeting. That isn't nice for modding, and it also makes upgrading the Unity version stressful, as the AssetBundle format can change over time.

As we don't need[0] all the features AssetBundles provide, we are opting to make our own cross-platform format that only supports the very few things we need.

The first task was defining a low-level format that only specifies blocks of bytes and an index. That was nice and simple to design, and I've left the save/loading code for another day, as it should be pretty straightforward.

From there, I'm making a format for storing meshes that is compatible with Unity's low-level mesh APIs. We want to use that low-level format as it allows us to create meshes across multiple threads and also do so with a minimal number of copies. I got a nice draft of that format finished in the morning and then started fleshing out the implementation of a zero-copy de/serializer[1].

Until now, we have used Unity's [blob asset reference](https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/blob-assets-create.html) system. However, it means we need a dependency on an additional Unity library, and I'm no longer comfortable with any unnecessary dependency on Unity systems (due to how Unity has acted this year[2]).

I think I'm about 70% of the way through writing the zero-copy stuff. Tomorrow, I'll start pushing meshes through this thing and see how it handles it. I'm sure I'll hit plenty of things I forgot up to that point.

As soon as meshes are working, I'm looking into how we will handle textures. I've been giving it some thought already, so it should be quick to make some progress.

I'll be sure to come back and update you once I've got some news to share on the texture progress. Especially as, from there, I should be able to dive into the UI that you folks will be interacting with!

I hope you're having a good day,

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] And actually we actively do not want all the features

[1] Feels weird to call it that when the point is that you don't need an explicit deserialize step as you just load it into memory and use it.. but meh. I can't recall a better term right now.

[2] This might seem moot after they rolled back their proposed fee changes for many folks, but I disagree, and I furthermore think the same applies to WOTC. The company is still run by mostly the same people, they still want money, and it stands to reason that sooner or later, they are going to try and extract it from us again. "Fool me once" can be overused, but it feels appropriate here.
