---
layout: post
title: TaleSpire Dev Log 435
description:
date: 2024-05-31 20:02:08
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good evening folks.

Today, I was looking into a really ugly slowdown when opening the "community" section of the asset library. I cracked open the profiler and, after poking around, found that calls to `new Texture2D` were insanely slow, easily 1ms each. I think it might be that either Unity is initializing the contents of the texture (which I'm immediately replacing) or can only make a certain number per frame.

The next version of Unity above the one we use does have an API call that allows making textures without initializing them, but the one we use doesn't. I tried upgrading the project to that newer version on the off chance everything would just work, but no. Some APIs critical to TaleSpire have changed, and refactoring the code that relies on them is not trivial.

So, I need to decide on a different approach next. I could write some C++ to make the textures without initializing them and call that from Unity. And/or I could spread the texture creation over several frames. The first option is trickier than I'd like as I don't think the conversion between Unity's GraphicsFormat and DX11's formats are documented anywhere (hopefully, I'm wrong; please leave a comment if you know!)

Before I could dig further into that, two things came up. One scheduled and one not. 

The first was a very disappointing case. TaleSpire did not recover from a lost connection properly and lost 2 hours of someone's work. This is terrible. So much of creation happens in the moment, and doing something for a second time is never quite the same. With our newer infrastructure, we now have much better logging, so we quickly tracked down the bug. What happened was that TaleSpire, at some point, lost connection to the backend (potentially due to a network issue.) Upon reconnection, we did not correctly re-register the ID of the board the player was working on. This put an invalid value into a temporary cache, which subsequently caused uploads to fail. The board was not destroyed, but the work for that session was lost. This bug has been around a long time, but now we've seen it, we know how to fix it. The first step is ensuring the server fails much sooner when it hits this case. That will cause the problem to manifest faster, and thus, the player won't be able to spend hours making things that we then lose. The second step is to fix TaleSpire itself. On reconnection to the backend, we must ensure that the board-id is registered before allowing play to continue. We will have a fix for this soon.

The scheduled portion of the afternoon was a planning meeting. @Borodust and I sat down and sketched out the next few months of backend development. We also took some time to review previous decisions and talked over some recent process problems we'd like to correct. It was nice, which is not something you usually hear in meetings. Working in a tiny team on something we really want to see exist helps a lot.

And that's all for today. I'll be back with more fixes and dev-logs in the next week.

Hope you enjoy the weekend. 

Ciao.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
