---
layout: post
title: Fps Counter with Temporal Functions
description:
date: 2014-04-14 00:00:46
category:
tags: ['lisp', 'cepl']
---

    (let ((count 0))
      (tdefun fps ()
        (incf count)
        ((each (seconds 1)) (print count) (setf count 0))))

It's simple but it makes me happy simply because it is written in term of time.

p.s. Oh yeah, as you may have guessed from this, temporal functions work now!
