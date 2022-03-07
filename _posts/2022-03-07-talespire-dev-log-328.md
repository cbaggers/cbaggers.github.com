---
layout: post
title: TaleSpire Dev Log 328
description:
date: 2022-03-07 18:29:39
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya!

Mac support has been going surprisingly well. Let's natter about it!

### Progress

The first order of business was pixel-picking. This requires custom native code as we need to use features of the graphics API not exposed by Unity. I had written the DirectX version [over a year ago](https://bouncyrock.com/news/articles/talespire-dev-log-254), but this time I needed something for [Metal](https://en.wikipedia.org/wiki/Metal_(API)). I haven't written any Metal code before, but I found [this article](https://developer.apple.com/documentation/metal/reading_pixel_data_from_a_drawable_texture) and [this code sample](https://github.com/Unity-Technologies/iOSNativeCodeSamples/blob/0c155b56b79649ed840e6cf35608040828e6d687/Graphics/MetalNativeRenderingPlugin/Assets/Plugins/iOS/MetalPlugin.m#L176) and felt pretty confident I could bodge something useful.

This turned out to be correct. I am very lucky that, at my previous job, I suffered through so much ObjectiveC development as it made the process much easier. After a bit of experimenting, the solution turned out to match the form of the DirectX one very closely, so the API for our plugin could remain the same.

It was fantastic to see picking come to life!

![picking on mac](/assets/videos/macPick0.gif)

As you'll have just seen, the cursors are HUGE! It turns out that Unity will try to match the cursor size as close to the source image as possible, so I need to go adjust the import settings.

Next up, I set myself to fixing a mistake in the build scripts[0], and then I needed to look at some more platform-specific code.

In Unity, if the game is in fullscreen mode, the reported resolution of the display is that of the fullscreen game. This is handy in some cases, but not for us. We want the full pixel resolution of the monitor that the game is currently on (according to the OS). This is easy to get from the [Cocoa API](https://en.wikipedia.org/wiki/Cocoa_(API)), and so this was another job for some native code.

With the pixel-picker under my belt, it was extremely simple to get this written.

Another thing that turned out to be super easy was supporting the `talespire://` URLs. Unity has a [simple setting](https://docs.unity3d.com/2021.2/Documentation/Manual/deep-linking-macos.html), allowing you to set up custom URL schemes. It was so easy that there is nothing else to say about it. So here's a picture instead:

![custom url support](/assets/images/macCustomUrls.png)

### Issues

There are plenty! The big one is that lots of shaders are broken. As an example, here is water:

![refreshing glitches](/assets/images/macBrokenWater.png)

The fps is also bizarrely low. Below I have a screenshot of the profiler. You can see that the work on the main thread is done in under 6ms, and the render thread is done by ~9ms. VSync is enabled, so I expect it to wait until 16ms before starting the next frame; however, it is waiting until ~25ms. Clearly, something is wrong or is missing from the graph.

This could be anything from a two-minute fix to a two-month one. We'll have to see!

![odd](/assets/images/macProfileWeirdness.png)

### What's next?

Clearly, there is still plenty to do before mac support can ship, but tomorrow I need to switch back to HeroForge and modding. I am also no shader expert, so I'll definitely have to kick this to Ree so he can work out what is breaking there.

I'm delighted with the 10Gb network gear we purchased to link my dev PC and the Mac. It has made cross-platform development so much nicer than what we have had in the past and has helped Mac support progress smoothly.

So that's it. I left out a bunch of smaller things for the sake of time, but I hope you're as excited as I am for where this is going.

Have a good one folks!


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] If `BuildPlayerOptions.locationPathName` does not end with `.app`, the dll containing the compiled burst jobs is not being copied to the plugins folder.
