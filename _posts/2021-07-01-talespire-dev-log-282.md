---
layout: post
title: TaleSpire Dev Log 282
description:
date: 2021-07-01 12:01:32
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks. For the last five days or so, I've kept saying to myself that i'd write the next dev-log "as soon as I finish *this* task," aaaand here we are :P

So let's dig into some of the stuff I've been poking at.

## Feature-Request Site

The #feature-requests channel on our discord has been incredibly useful. Still, a while ago, we gained enough of you lovely folks that we've outgrown it as a viable solution for managing feature requests.

Since then, the moderators have done a ton of work exploring options we have for moving that somewhere more appropriate. We are not opening it today, but we'll soon be moving it and the roadmap over to HelloNext.

We've going through every request from talespire.com/faq and the last feature roundup and entering them into that system.

We are not ready yet, but we will put the #feature-request channel in read-only mode when we are. This way, nothing is lost, but new folks won't be confused about where to post.

We will have more news about this in the coming weeks.

## Bookmarks

Back in the code, I've been experimenting with bookmarks. Bookmarks are markers that have been given a name. We want you to be able to easily search and jump to any bookmark in the campaign.

I started with making a basic panel to hold the bookmarks so we could start experimenting with behavior. In the pic below, neither the logic nor the graphics are final. But you can see I was messing around with how to visualize bookmarks in the current board.

![wip0](/assets/images/wipBoardPanel0.gif)

From this, we decided that the bookmarks should be integrated into the boards panel, with the boards acting like folders. You can see the WIP of this in these images.

![wip1](/assets/images/wipBoardPanel1.gif)

![wip2](/assets/images/wipBoardPanel2.png)

This is going well. The next step is to add some new functionality to the server to support things like campaign-wide bookmark search.

## Internal systems documentation

Along with this, we've also needed to start looking at housekeeping.

We have a growing codebase, and to keep things running smoothly, we need both Ree and I to be able to drop in easily and get working. To help with this, we are making some internal documentation.

While doing this, I've found stuff that I'm reluctant to document as they require some cleanups. Some of which have been on my todo list since the release.

To that end, I finally dove back into the serialization code and got the API to a nice place. This touched a **large** amount of code and so that it took a couple of days to update everything and get it passing all the tests again. We really don't want to start breaking boards at this point.

From now on, I'm going to try to put aside one day a week where I only work on things that benefit us behind the scenes.

To get good performance, we have had to make alternatives to some of Unity's systems, and our versions lack the visuals and tools that make working with them enjoyable. Focusing here will make our lives easier and thus make it easier to get fixes and features to you.

## NDI

One day last week, I needed a change of scene, so I decided to look back into [ndi](https://en.wikipedia.org/wiki/Network_Device_Interface) support. I was hopeful that my knowledge of Unity has increased since last time, and I thought I could get it closer to shippable.

Alas, the plugin now requires .NET Standard 2.0, and for various reasons, TaleSpire uses .Net Framework 4.* (Damn Microsoft's naming schemes to the pits of hell). Switching TaleSpire to .NET Standard 2.0 broke several things[0], so suddenly, this task stopped being a nice way to unwind.

This will have to wait for another day.

## Unity on Linux [WARNING: NOT OFFICIAL SUPPORT - PLEASE READ BELOW]

Multi-platform support is NOT coming yet. But it is our long-term goal, and when it does, we want all your mods to work there too. Part of this means getting an understanding of how Unity's AssetBundle format works on these platforms.

Also, I prefer working on Ubuntu, so it was a great time to try out the Linux port of Unity.

The tests went well. I was able to get a rather broken version of TaleSpire running on Ubuntu and prove that we can use AssetBundles that were made for Windows in the Linux build (because of how we handle shaders internally). Next, we'll have to do a similar test on macOS and see how it behaves.

There is still a lot of work to do to make the build work properly. I'll mention just two for now.

We use Window's 'named pipes' feature to handle custom URL schemes. We will need to make something similar. This was a real pain to get working on Windows, so I expect no less elsewhere :P

The pixel-picking system makes use of a custom c++ plugin we made to be able to get results back from the GPU without hangs. We will need to write variants of this for each platform.

Now, I'm sure a few of you have been yelling at the screen to just use wine/proton to support Linux, and we are well aware of these. TaleSpire seems to work great out of the box with Proton, except for the custom URL scheme. This, too, can be fixed, but we'd want to fix that wrinkle in the experience before we'd consider supporting it.

Also, we simply aren't ready to take on the extra work that supporting multiple platforms requires. We will get there, but it's something for another day. For now, I'm excited to see the path ahead.

## Bugs

As usual, there are also bugs to be looked at. We still have cases of board corruption being reported, and so these take top priority. Not all of them turn out to be corruption, but they still are worth squashing.

## I think that's most of it

We naturally have plenty of other things brewing. This post was just my stuff, and the rest of the team has been just as busy. We also have things cooking that we aren't ready to talk publicly about. It's gonna be fun :)

Until next time folks,

Have a good one!

[0] Including the custom URL scheme and some HTTP connection stuff
