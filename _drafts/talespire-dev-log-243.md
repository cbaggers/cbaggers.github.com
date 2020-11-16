---
layout: post
title: TaleSpire Dev Log 243
description:
date: 2020-11-16 23:36:41
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi!

On Sunday I was reading about [Unity CommandBuffers](https://docs.unity3d.com/ScriptReference/Rendering.CommandBuffer.html) as I need to rewrite the code that handles the cutaway effect. At the bottom of the page was a link to [this blog post](https://blogs.unity3d.com/2015/02/06/extending-unity-5-rendering-pipeline-command-buffers/) where something caught my eye. They were talking about custom deferred lights. This was exciting as lights are the one place where tiles still need to use Unity GameObjects.

GameObjects have been our nemesis when scaling up TaleSpire as they are (relatively) slow to spawn, use managed memory, and we can only interact with them from the main thread. Currently, to avoid lag, we have to spread light spawning over multiple frames and work pretty hard to apply transform updates to the GameObjects from the Spaghet scripts which can modify them.

If we had a way to draw lights without GameObjects we could significantly simplify a lot of code. The temptation was too great and so today I spent the whole day working out how to do this.

The good news is that it worked. Here is a very ugly test.

![firstCustomLight](/assets/images/firstCustomLight.png)

On the left we use a standard Unity light and on the right the new approach. They should look the same. The reason for the pattern in the light is that I knew that we needed to support [light cookies](https://docs.unity3d.com/Manual/Cookies.html) and so I included that in the test. The non-cookie case should be simpler.

Ok back to the blog. The author has included a fanstastic example project which was the first concrete proof that this should be possible. I had one extra complicating factor however, I didn't want our lights to look any different from the Unity ones. To me this meant trying to drive the built in Unity Shaders somehow. Due to my inexperience with this I had a bunch of false starts, however I eventually was able to understand the general flow of the deferred light rendering process. There is a good, though terse, summary of the approach [here](https://kayru.org/articles/deferred-stencil/).

Things were looking promising, however, try as I might, I could never get the built in shaders to have the ZWrite or Stencil settings I needed. After a lot of failed experiements I simply copied the bits I needed into a seperate shader so that I could apply the flags myself. This process saw me running the test, checking the frame debugger to see what state was wrong, trying to correct it, repeat.
