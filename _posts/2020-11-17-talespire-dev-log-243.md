---
layout: post
title: TaleSpire Dev Log 243
description:
date: 2020-11-16 23:36:41
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi!

On Sunday, I was reading about [Unity CommandBuffers](https://docs.unity3d.com/ScriptReference/Rendering.CommandBuffer.html) as I need to rewrite the code that handles the cutaway effect. At the bottom of the page was a link to [this blog post](https://blogs.unity3d.com/2015/02/06/extending-unity-5-rendering-pipeline-command-buffers/) where something caught my eye. They were talking about custom deferred lights. This was exciting as lights are the one place where tiles still need to use Unity GameObjects.

In our quest to improve TaleSpire's performance, GameObjects have long been our nemesis. They are (relatively) slow to spawn, use managed memory, and only interactable from the main thread. To avoid lag, we have to spread light spawning over multiple frames and work pretty hard to apply updated transforms to the GameObjects from the Spaghet scripts that can modify them.

If we had a way to draw lights without GameObjects, we could significantly simplify a lot of code. The temptation was too great, so I've spent the whole day working out how to do this.

The good news is that it worked. Here is an ugly test.

![firstCustomLight](/assets/images/firstCustomLight.png)

We use a standard Unity light on the left, and on the right, the new approach. They should look the same. The reason for the pattern in the light is that I knew that we needed to support [light cookies](https://docs.unity3d.com/Manual/Cookies.html), and so I included that in the test. The non-cookie case should be more straightforward.

Ok, back to the blog. The author has included a fantastic example project, which was the first concrete proof that this should be possible. I had one extra complicating factor; however, I didn't want our lights to look any different from the Unity ones. To me, this meant trying to drive the built-in Unity Shaders somehow. Due to my inexperience with this, I had a bunch of false starts, but I eventually understood the general flow of the rendering process. There is a good, though terse, a summary of the approach [here](https://kayru.org/articles/deferred-stencil/).

Things were looking promising but, try as I might, I could never get the built-in shaders to have the ZWrite or Stencil settings I needed. After many failed experiments, I simply copied the bits I needed into a separate shader to apply the flags myself. This process was easier said than done, and a lot of time was spent in the frame-debugger.

It's all looking very promising, but there is still time for it to all go wrong :P

The next step is to make sure this works with multiple lights. Then I can extend this to support spot-lights and more of the standard point light settings. Once those are done, we will change how TaleWeaver packs light data, and then finally, we can refactor the light batch in TaleSpire to use this new system. I wonder if we can get some idea of performance without doing all of this… we shall see.

Alright, that's all for today. 
Seeya around!
