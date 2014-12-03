---
layout: post
title: Soon there will be a new Stage
description:
date: 2014-12-02 23:50:22
category:
tags: ['lisp', 'cepl']
---

After having merged in all the nice changes to foreign arrays I have decided to start looking at what is probably the biggest hole in the glsl compiler right now, Geometry Shaders.

I'm beavering away on some refactoring and code cleanup to make this tractable, which is going rather well. There should be no major changes to the compiler needed as I added all the geometry shader specific functions ages ago. I think it will mainly be glsl 'out' variable type transformations. I will also need to be able to specify the geometry type (tris, points, lines) in the context as that logic will be needed for proving correctness in the geometry stage.

I am not too fussed about adding Tessellation shaders urgently, mainly because I haven't learned how to use them yet.

I am also musing over the idea of supporting the use of 'declare' to specify shader argument types and the context. So that the following

    (defvshader test ((verts pnc) &uniform (i :int) (origin :vec3) (cam-to-world :mat4)
                      &context :version 330)
      ...shader code here)

Would instead look like:

    (defvshader test (verts &uniform i origin cam-to-world)
      (declare (pnc verts) (:int i) (:vec3 origin) (:mat4 cam-to-world)
               (context :version 330))
      ...shader code here)

Which is more lispy...but as types are mandatory I'm not sure how I feel about this. Does this also mean I specify 'out' variables here?

** NOTE: At the start of this post I knew what I was going to blog about, but now I am off on a mental wander. So sorry for the round about nature of the rest of the post :) **

It does make the main signature clearer, especially where array types are concerned.

    (i (:int 3))

The above is the current signature for an array of 3 ints, get a few of these in the argument list and it gets messy.

But this also means I will have to do the same for labels...

    (labels ((test ((i :int) (y (:int 3)))
               (+ i (aref y 0))))
      (test 1 some-int-array))

..becomes..

    (labels ((test (i y)
               (declare (:int i) ((:int 3) y))
               (+ i (aref y 0))))
      (test 1 some-int-array))

Hmm, not too bad, the first example is a jumble... how about 'let' forms?

    (let ((x 1)
          ((y :int) 2)
          ((z (:int 3)) some-array))
      (some-func x y z))

Notice how the types are optional as they can be inferred, so in declare style

    (let ((x 1)
          (y 2)
          (z some-array))
      (declare (:int y) ((:int 3) some-array))
      (some-func x y z))

Ok now in this case it is a BIG improvement to the actual readability of the declarations themselves.

The big worry for me though is giving you two places to look for one piece of information when reading the code. What you want to know is "What is x" and you have to look at for it's declaration (so you can know it's scope) and the declare (to know it's type).

The other possibility is to be super un-common-lispy and use some special reader syntax that is ONLY valid in bindings.

    (labels ((test ([i :int] [y (:int 3)])
               (+ i (aref y 0))))
      (test 1 some-int-array))

    (let ((x 1)
          ([y :int] 2)
          ([z (:int 3)] some-array))
      (some-func x y z))

Ugh...even writing it down feels wrong. Nah, scrap this idea. I think the declare style is growing on me but I will need to give this some time. I'd appreciate any ideas on this one.

Ciao