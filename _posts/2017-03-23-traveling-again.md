---
layout: post
title: Traveling Again
description:
date: 2017-03-23 11:29:50
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Turns out I booked a bunch of small trips last year and they all are around now. Sorry for the late update.

I got back to coding yesterday and have been working on the compiler. Since the talk I have some great support from [djeis97](https://github.com/djeis97) and [jobez](https://github.com/jobez) on issues in CEPL & Varjo. One big thing I realized was more broken that expected was Geometry Shaders, so that is current top priority.

The prerequisites for fixing this are better array support in Varjo & support for interface blocks, so whilst traveling I was reading the GLSL spec again. Last night I added checks to make sure you are not trying to access an uninitialized variable and then started on adding lisp's `make-array` function.

As I currently don't track `const`'ness in Varjo I can't validate a bunch of the array rules that the GLSL requires. However those are errors that will be caught by the driver's own glsl compiler so I'm not worrying about those too much yet.

That's all for now
Peace
