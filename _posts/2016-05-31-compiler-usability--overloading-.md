---
layout: post
title: Compiler usability & overloading
description:
date: 2016-05-31 10:00:27
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

This last week has been fairly chill.

Whilst I was at the conference I had a couple of folks who do graphics research take an interest in CEPL and so I decided I should put a little time into making the compiler a little easier to use. The result is a function called v-compile that takes the code for a stage as a list and returns the compiled result. Using it looks like this:

```
VARJO> (v-compile '((a :float)) :330
                  :vertex '(((pos :vec3))
                            (values (v! pos 1) a))
                  :geometry '(((hmm (:float *)))
                              1.0)
                  :fragment '(((hmm :float))
                              (labels ((fun ((x :float))
                                         (* x x)))
                                (v! 1.0 1.0 hmm (fun a)))))
```

The first two arguments are the uniforms `((a :float))` (in this case one float called `a`) and the glsl version you are using (`330`)

You specify the stage as a keyword and then provide a list. The first element of the list is the list of arguments to that stage. e.g. `((pos :vec3))` the rest of the list is the code for that stage e.g. `(values (v! pos 1) a)`

I also took all of the work I did expanding the glsl spec and use it in the compiler now. At compile time my compiler reads the glsl-spec and populates itself with all the function and variable definitions. This also means that varjo now works for all version of glsl YAY!

I also added **very** tentative support for geometry and tesselation stages. I didnt have time to learn the spec well enough to make my compiler check the interface properly, but instead it just does very basic checks and gives a warnign that you should be careful.

Finally I made it easy to add new functions to the compiler and made CEPL support gpu-function overloading. So now the following works.

```
(defun-g my-sqrt ((a :int))
  (sqrt a))

(defun-g my-sqrt ((a :vec2))
  (v! (sqrt (x a)) (sqrt (y a))))
```

Seeya folks
