---
layout: post
title: Browsing remote servers with eshell
description: 
category:
tags: ['']
---

As a budding Emacs nerd I'm looking at replacing my current awesome terminal emulator <a href="http://www.tenshu.net/p/terminator.html">Terminator</a> (it's in the Ubuntu repos) with a shell within emacs.

I had a play with eshell and it worked well, but I wanted to be able to use a feature called find-file-at-point which opens the file your cursor is currently over in the buffer.

Turns out this is pretty damn easy:
cd /ssh:username@servername:/path/you/want

Hit return, type in your password and you in!