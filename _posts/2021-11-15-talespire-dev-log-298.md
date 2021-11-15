---
layout: post
title: TaleSpire Dev Log 298
description:
date: 2021-11-09 23:40:28
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks, time for another dev-log :)

## HeroForge work continues

The first task was to replace the use of BlobAssetReference for serializing the HeroForge asset data, as yesterday we discovered that it was far too slow (~100ms on my machine).

That went smoothly and removed the most significant lag in our code. This allowed us to see all the stuff that was still too slow. From there, the bulk of the work was moving things to jobs. And making sure that, whenever we did have to do something on the main thread, we did it fast.

With asset processing/saving improved, I moved on to asset loading. The work was very similar until something cropped up that was rather surprising…

![slow texture upload](/assets/images/slowUpload.png)

…Unity was taking a very long time to upload the textures. At first, I thought we were trying to access the texture's data too soon after creation, so I put a delay between those steps. I think it helped a little, but the spike in the perf graph still remained.

I'll spare you the details of all the tests and head-scratching, but I eventually stumbled on something by accident:

![fast texture upload when busy](/assets/images/confusingTimings.png)

The first spike is the load with slow texture upload. All the little spikes are from running the same code in rapid succession. It seems that, after a delay, the first texture upload is slow, but uploads within a couple dozen frames of each other are fast.

I could make some wild guesses about what is happening, but to be honest, I just don't know. For now, it's enough to know that it's not going to choke when batch processing, but it is rather unsettling.

There are two other spots where the loading code is slower than I'd like.

The first is that opening the asset file takes a couple of milliseconds. We can just move that out to the job that reads from that file.

The second is that the Texture2D constructors take a few milliseconds to run. Unfortunately, there isn't I can do about that one as it's not our code :(

Tomorrow I'll start moving this into TaleSpire.

## macOS support

About two weeks ago, Unity 2021.2 shipped. This is the first build of the editor to officially support Apple Silicon.

We had already tested TaleSpire in the 2021.2 beta and, on Windows, all looks good. This means we will probably upgrade the project in the coming weeks.

TaleSpire on macOS is still, of course, totally broken. But this was already known, and we covered the details [back in this dev-log](https://bouncyrock.com/news/articles/the-road-to-multiplatform). We can, however, prepare our development environment. We are currently waiting on a fix to [a Rider bug](https://youtrack.jetbrains.com/issue/RSRP-486608) before we can code comfortably there[0]. However, a more ideal solution would be cross-compiling from our primary dev environment on Windows.

I've done my fair share of pushing builds across the network to other Windows test machines, and the time it takes to move a few GB really makes iteration times painful. If you've done much coding, I'm sure you know just how much difference iteration time makes to flow and thus how fast you can get things done.

Rather than replicate that *joy* on macOS, we have ordered some Sonnet 10Gb gear and will be testing to see if that makes for a tenable experience.

I'm rather excited about that!

## That's your lot

Ok, that's all for tonight.

Hope you've had a good day.

Peace.




*[0] I'm a big ol' emacs nerd and would have loved to stay, but omnisharp or its integration are just too slow. I tried all the usual suspects, but Rider has unfortunately been the best for me by a fair margin so far.*
