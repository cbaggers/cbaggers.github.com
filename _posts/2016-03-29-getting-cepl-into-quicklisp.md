---
layout: post
title: Getting CEPL into Quicklisp
description:
date: 2016-03-29 15:14:54
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Sorry I havent been posting for ages. There has been plenty of progress but I have been lax writing it up here.

The biggest news up front is that CEPL is now in quicklisp, this has been a long time coming but now it's much now easier to get CEPL set up and running.

To do this means cleaning up all the cruft and experiments that were in the CEPL repo. The result is that like GL CEPL has a host that provides the context and window, the event system is it's own project, as does the camera, and much else is cleaned and trimmed.

CEPL can now potentially have more hosts than just SDL2 though today it remains the only one. I really want to get the GLOP host written so we can have one less C dependancy on windows and linux.

More news coming :)
