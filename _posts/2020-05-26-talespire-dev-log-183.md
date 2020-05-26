---
layout: post
title: TaleSpire Dev Log 183
description:
date: 2020-05-26 12:59:08
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, time for another log.

We have a content update dropping soon, but also in there is a potential fix for those who have been unable to play due to their machine not supporting SSE4. This limitation was due to older versions of the Burst compiler not supporting earlier SIMD instructions. We had wanted to upgrade the Burst compiler package previously, but we found a bug in that package that blocked us. We (reported it to Unity not long back)[https://issuetracker.unity3d.com/issues/the-sizeof-of-struct-with-structlayout-returns-a-smaller-value-in-the-burst-compiled-code-compared-to-the-mono-compiled-code] and they have already shipped a fix! With the packages updated, we should now support fallback to SSE2 (which every x64 machine supports).

My laptop, after holding on for nine years, finally started breaking the other day. My new one has just arrived. That means I'm going to lose at least a day to swearing at windows for being such a massive, invasive, pain in the ass and setting up Linux so I can get server work done in a more agreeable environment.

I've also started on the server changes to support creature scale. These are going well, and I will begin integrating the changes on the client-side soon.

Alright, that'll do for now. Back to the laptop for me.

Ciao
