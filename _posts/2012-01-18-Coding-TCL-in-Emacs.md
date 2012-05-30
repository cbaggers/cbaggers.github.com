---
layout: post
title: Coding TCL in Emacs
description: 
category:
tags: ['']
---

This is just a quick post.
At work I'm having to do a lot of TCL development and having recently become a convert to Emacs I obviously want to use it for everything!

The shortcuts are a bugger to remember so I thought I'd dump them here so I know where I can find them.

From <a href="http://www.rgrjr.com/emacs/advanced.html#tcl-mode">http://www.rgrjr.com/emacs/advanced.html#tcl-mode</a>

Commands:

M-x tcl-mode
M-x inferior-tcl (C-c C-t in a Tcl source buffer)
Tcl editing:

C-c TAB (tcl-help-on-word)
C-c C-c (comment-region)
Tcl editing support for inferior Tcl:

C-c C-t (inferior-tcl) creates one.
C-c C-s (switch-to-tcl) switches to the inferior Tcl process buffer (but does not create one).
C-c C-x (tcl-eval-region)
C-M-x or C-c C-v (tcl-eval-defun)
C-c C-f (tcl-load-file) loads a Tcl file into the inferior Tcl process

Awesome! The eval region has to be the thing I love most from writing lis pin emacs so its great to have it for TCL.

Anyhoo thats all!