---
layout: post
title: Cepl on OSX
description:
date: 2014-04-15 17:29:22
category:
tags: ['lisp', 'cepl']
---

This has been a massive pain in the arse and still isn't working in the way I would like it but there is some progress so it is worth testing.

First off, while I love sbcl, I cannot get sdl2 to create a fully working window. I get a window with no border or one which does not receive input events.

So instead I tried clozure (ccl) and have had some success.

First cepl:repl now wraps it's contents in an sdl2:in-main-thread form. So you can continue to use this as usual.

Next you need to wrap the code that contains your main loop in an 'in-main-thread' form. In the case of the provided examples you can just write (sdl2:in-main-thread () (run-demo)).

This gives you an active window that is decorated and accepts events which is wonderful. The only problem so far is that #'update-swank doesn't work and so you cant use slime's repl while the demo is running. This is a deal breaker for me, as that ruins my workflow so I  need as solution to consider using osx for any cepl development.

Hopefully a solution to this will emerge soon. I then really need to go clean up the code surrounding this fix.

Here's hoping

[edit]
Seems the repl in the inferior-lisp buffer (or shell) is active and usable. However the repl buffer doesnt yet. Can anyone with more experience with ccl help me out here?