---
layout: post
title: And just like that...
description: 
category:
tags: ['']
---

..All is right with the world (of emacs) again.

Turns out sbcl + slime (+ quicklisp?) really doesnt play well if autopair is enabled.

You will know you have  the problem if whenever you cause an exception in the repl you are unable to use the minibuffer at all without seeing the following:
<em>error in process filter: Wrong type argument: characterp, nil</em>
Or something very similar.
This effectively means killing emacs and restarting, as killing/switching buffers, closing emacs, loading files, scratching your bum are all inoperable.

Anyhoo how this helps you if it's caught you out.
Ciao