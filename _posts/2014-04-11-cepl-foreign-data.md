---
layout: post
title: Cepl Foreign Data
description:
date: 2014-04-11 10:21:27
category:
tags: ['lisp', 'cepl']
---

Up until now cepl has been putting it's own wrapper around [cffi](https://github.com/rpav/cl-autowrap) data in order to make it easier to manage and play with in lisp.

I wanted to start supporting more complicated foreign data and so was just about to start writing when I remembered cl-autowrap. If you are a lisp person and you haven't checked autowrap out yet then please do, it is a fantastic wrapper generator which given this..

    (c-include "file.h")

..Will write an entire lisp wrapper around the foreign library. It also provides hooks (via [trivial-garbage](http://common-lisp.net/project/cffi/) ) to allow the CL's garbage collector to manage your foreign data.

A little digging around in the back-end (with some great help from rpav, cheers man!) of autowrap has shown me that there is more than enough data back there for me to be able to write something like this:

    (make-available-for-gpu 'some-type)

And then have the type fully integrated into my glsl compiler.

I do need to augment the c-autowrap slightly as I want to be able to recieve cepl's own c-array objects instead of autowrap's. But this is certainly do-able and I'll have a good look at it next week.

Righto folks, I need coffee

Seeya