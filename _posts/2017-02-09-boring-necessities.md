---
layout: post
title: Boring Necessities
description:
date: 2017-02-09 21:47:47
category:
tags: ['lisp', 'cepl']
---

This week (and a bit) has felt like a bit of a blur as I've mainly been working on boring stuff that just needed doing. I'll be honest, I can't remember what I did so I'll just go spelunking through my commits and see what I find.

# lisp-games-cross-test

I started yet another project :p. This came out of me feeling the uncertainty you get when you have written a good chunk of that stack you use (and don't have enough tests). It's stupid to have written this thing which is meant to be nice for creating things and then be stymied by and lingering sense that something, somewhere must be breaking.

One thing I really wanted to be sure about was that my maths library [rtg-math](https://github.com/cbaggers/rtg-math) was solid. 

As I didn't want to rely on my own (very limited) knowledge I decided to make a basic fuzz testing setup where I compare my library against other math libraries written in Common Lisp.

I built on top of the excellent [fiveam](https://common-lisp.net/project/fiveam/) testing library and made a fairly nice system for defining comparative tests across several libraries. It takes into account that a library's features will not exactly overlap with another. I'm happy with it so far but it's not ready for github yet.

Anyhoo I set it to work and found that a lot of stuff is working just fine. One function (the matrix to quaternion function) was broken. At first I stupidly blamed the c++ I based the function on until, after many re-reads, I noticed that the `[]` operator had been overloaded to index from `x` and ignore the `w` component altogether. This meant I had a bunch of 'off by one' indexing errors in that function :facepalm:.

However I did find a typo in the book's code which has now been [fixed](https://github.com/jvanverth/essentialmath/issues/57)! So that's a nice side effect of this work.

I then got massively mistake about the api of another library I was comparing to (sorry axion). The process of being gently schooled by psilord on the `#lispgames` irc was incredibly helpful but did send me into frequent 'What if my entire api is wrong' panics.

Luckily he provided me with some [excellent](https://fgiesen.wordpress.com/2012/02/12/row-major-vs-column-major-row-vectors-vs-column-vectors/) [links](https://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/geometry/row-major-vs-column-major-vector) and I'm slowly getting a better grasp of the terminology. 

Luckily, as my library is heavily based on the c++ code from the text book I learned from, my library is fine and then only problems were (and to some extent remain) with my brain :D

# RTG-Math

Whilst working on the above I also got a friendly jab that my vector math was too slow. It turns out the 'jab-er' in question was comparing his destructive (mutable) api with my non-destructive one. This is kind of understandable as I didn't have a destructive api to compare against.. but of course I fixed that `>:)`

Below is the (very basic) test I did to check how it performs:

```
;; First we make a list of 1000 random vector3s
;;                                    ↓↓
RTG-MATH> (let ((to-add (loop :for i :below 1000 :collect 
                           (v! (- (random 2f0) 1f0)
                               (- (random 2f0) 1f0)
                               (- (random 2f0) 1f0))))
                (res (v! 0 0 0)))
            ;;
            ;; Here we make the list circular
            ;;     ↓↓
            (setf (cdr (last to-add)) to-add) 
            ;;
            ;; Then we iterate 100 million times adding the elements of
            ;; the circular list into the variable 'res'
            ;;    ↓↓
            (time
             (loop :for i :below 100000000 :for e :in to-add :do
                (v3-n:+ res e)))
            res)
            
Evaluation took:
  0.748 seconds of real time
  0.748000 seconds of total run time (0.748000 user, 0.000000 system)
  100.00% CPU
  2,297,194,923 processor cycles
  0 bytes consed 
  
#(-1248991.9 2382024.5 358581.84)
```

Not too shabby! `bytes consed` in lisp means 'memory allocated' so we see this only mutating the 
vectors already created.

The test is pretty rough and ready however the list is long enough that it wouldn't just fit in a cache line, and the random calculates are outside the loop as they the slowest part of my original test.

In the process of making these destructive versions of the API I have been going through the majority of the codebase adding type declarations to everything I could. I still have some optimization work and cleanup to do but it's mostly giving the compiler the information it needs to do it's job.

Optional typing is fucking great

# Nineveh 

[Nineveh](https://github.com/cbaggers/nineveh) is intended to be a 'standard library' of gpu functions and helpers for CEPL. It has code that doesn't belong in a GL abstraction but is really useful to have. Currently it's not in quicklisp (the lisp package manager) as it's still too small & too in flux however progress is being made.

The main addition this week was a `draw-tex` function which will take any texture or sampler and draw it to the screen. Simple stuff but it has a bunch of options for drawing in certain portions of the screen (top-left, bottom-right etc) scaling, rotation, drawing cubemaps in a nice 'cross' layout and maintaining aspect ratios while doing this.

Also [Ferris](https://www.twitch.tv/ferrisstreamsstuff) helped me find some (really nasty) bugs in one of the functions in the library which was ace.

# PBR

I spent yesterday evening at Ferris' place where I got a bunch of help with some artifacts I was seeing in my code. It helps so much to have someone who knows what certain things should look like. When it comes to graphics coding I really lack that.

In the process of this I found the previous mentioned bug in nineveh, a bug in my compiler (`let` wasn't throwing an error for duplicate var names) and added a way to support `uint` literals.

This has made me much more confident in the code, and with where the rest of artifacts I'm seeing are probably coming from. HOPEFULLY will have some new screenshots soon.

# Sleep

I haven't been getting enough, so I'm going to bed.

Seeya!
