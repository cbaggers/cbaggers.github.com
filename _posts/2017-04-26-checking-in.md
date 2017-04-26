---
layout: post
title: Checking in
description:
date: 2017-04-26 10:55:51
category:
tags: ['lisp', 'cepl', 'varjo']
commentIssueId:
---

So this week I don't have much technical to write about, the main things I've been doing are
- Bug-fixing/refactoring
- Getting everything into master and then finally
- Getting the next release ready.

This month's release will bring a huge slew of changes into the compiler, starting with all the geometry shader work I've been doing. I've also landed the new backwards compatible host API for CEPL which brings multiple window support among other things.

In the next 2 weeks I want to have finished my latest cleanup of Varjo and added support for tessellation stages.

After this I've reached an interesting place with regards to CEPL as I think the major features will be done. I've still got a few things I want support for before I call it v 1.0:

- Scissor
- Stenciling
- Instanced Arrays
- Multiple Contexts

And each of those seem like fairly contained pieces of work. Even though there are things in GL I haven't considered supporting yet (like transform feedback) I don't think I've ever felt this close to CEPL being 'ready'.. It's an odd feeling.

After that I'd like to focus of stability, performance and on [Nineveh](https://github.com/cbaggers/nineveh) which is going to be my 'standard library' of GPU functions and graphics related stuff.

Until next week,

Peace
