---
layout: post
title: It's Just Pretty
description:
date: 2014-11-25 00:21:00
category:
tags: ['lisp', 'cepl']
---

First some code!:

    > (setf c (make-c-array #(1 2 3 4)))
    #<C-ARRAY :element-type :UBYTE :dimensions (4)>

    > (setf g (make-gpu-array #(1 2 3 4)))
    #<GPU-ARRAY :element-type :UBYTE :dimensions (4) :backed-by :BUFFER>

    > (setf g (make-texture #(1 2 3 4)))
    #<GL-TEXTURE-1D (4)>

    > (texref *)
    #<GPU-ARRAY :element-type :R8 :dimensions (4) :backed-by :TEXTURE>

I have tweaked the make-\*-array and make-\*-texture commands so that, if you only pass in a lisp data structure, cepl will try and find a the smallest gl type that can be used across the whole data-set.

For example:

    (make-c-array #(1 2 3 4)) ;; <- element type will be :ubyte

    (make-c-array #(1 -2 3 4)) ;; <- element type will be :byte

    (make-c-array #(1 -2 200 4)) ;; <- element type will be :int

    (make-c-array #(1 -2 200 4.0)) ;; <- element type will be :float

    (make-c-array #(1 -2 200 4.0d0)) ;; <- element type will be :double


This is one of those nice cases where you get to make repl exploration more fun with no cost to speed. The reason for this is no high level code will be using this inference as it would be stunningly stupid to pay that cost for no reason. All file loading (which is the most common case) will have the type name anyway so it can just be passed in as usual.

Little details, but pleasant ones