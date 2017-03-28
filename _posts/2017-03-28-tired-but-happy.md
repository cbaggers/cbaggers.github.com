---
layout: post
title: Tired but happy
description:
date: 2017-03-28 09:52:45
category:
tags: ['lisp', 'cepl', 'varjo']
commentIssueId:
---

I'm tired right now but also really happy with the reception the video got.

I've had a number of PRs to CEPL & Varjo with fixes for various things. Of special note is a chap called `djeis97` who has been given me a pile of help with issues in my geometry shader generation. He's been using the inline glsl features of CEPL too which is awesome.

In order for geometry shaders to work properly I need to do the following:
- Improve CEPL's handling of arrays
- Generate interface blocks between shader stages
- Fix some hacks around `return` & `out` values
- Add some helpers for emitting vertices.

I've made some progress on the arrays in the last week and will be starting on interface blocks asap.

I've also had one chap who works on a project called [sketch](https://github.com/vydd/sketch) decide that he has had enough with dealing with opengl directly and wants to build his project on top of CEPL. This is _exactly_ the kind of project I've wanted to see built with CEPL, stuff that has a clear set of trade-offs and makes making stuff super simple. His current approach supports multiple windows however, and CEPL did not, so I have been getting that in place. This has meant (even though Im not implementing yet) looking at multiple GL contexts and threading.

CEPL has no business with your threads and expects you to use them wisely, `sketch` however was built on top of [sdl2kit](https://github.com/lispgames/sdl2kit) which handles threading for you. This mismatch with CEPL is a little problematic and, as I don't yet have any libraries for helping with the threading situation, I'm going to make a shim to make sdl2kit drive CEPL. It'll be butt ugly but will hopefully be enough for now.

It's the European Lisp Symposium in Brussels next week so it would be awesome if I had something new to give a lightning talk on..or even just to show of with :)

At some point I need sleep too.

Ciao!
