---
layout: post
title: Buffer Backed Textures
description:
date: 2014-07-28 09:26:48
category:
tags: ['lisp', 'cepl']
---

Last night I was musing over rendering graphs into textures and I was interested in how to write just a couple of pixels to the texture without reuploading the image. Now I can use gl's 'copy sub image' functions but looking through my copy I saw I hadn't implemented buffer backed textures; this gave me a great excuse to finally get it done!

In cepl buffer backed arrays would more accurately be named array-backed-textures as we already have the gpu-array abstraction over buffers. This works in my favour though as currently the abstraction for textures in cepl is as follows:

* A texture is a structure that holds a number of images. You use texref (like aref) to get at one of the images.
* Images are just arrays so they are exposed as texture backed gpu-arrays.

So calling texref on a regular texture gives you a texture-backed gpu-array and calling texref on a buffer-backed texture give you a buffer-backed gpu-array.

So to get change one pixel of a texture to the color red we can do this:

    (with-gpu-array-as-c-array ((texref texture) :tempname arr)
      (setf (aref-c arr 10 10) (v! 1 0 0)))

I have been coding a lot of c++, java and c# recently and so coming back to lisp and doing things like the above just makes me so damn happy! I love this language.