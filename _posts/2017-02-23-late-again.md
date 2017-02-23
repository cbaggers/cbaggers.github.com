---
layout: post
title: Late again
description:
date: 2017-02-23 17:59:01
category:
tags: ['lisp', 'cepl']
---

I have travelled to see family this week so sorry for the late writeup.

This week went pretty well. I went through the [PBR example](https://github.com/tuccio/IBLGGX) I had been using line by line making notes and then tried to match my implementation to theirs.

I had made some progress but still the colors looked washed out, something must have been puting the color values way out of wack. Finally I saw it, I had been adding a color I should have been multiplying. A facepalm and a tweak later color started looking like this 

![much better](https://raw.githubusercontent.com/cbaggers/lark/no-ecs/fuckin_finally.png)

I'm still not convinced that the example's approach for creating the final color is correct, however this is plenty good enough for now.

With that done I revisited my UI code (which uses nuklear) and found that it was broken...shits.

A bunch of git archeology told me the issue was related to my GL context caching. Double shits

..and then that the crash occured because I had made CEPL's handling of the context more robust. At this point a drink was in order, but after that, fixing!

The result was that I hadnt been unbinding VAOs correctly when rendering and so then subsequent buffer unbindings had been captured in the VAO. This sucks but was a 1 line fix and CEPL is much better for it.

Theres other stuff going on buts they are not worth yakking about right now. 

Seeya
