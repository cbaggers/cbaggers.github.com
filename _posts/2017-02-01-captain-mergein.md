---
layout: post
title: Captain Merge'in
description:
date: 2017-01-31 23:07:59
category:
tags: ['lisp', 'cepl', 'varjo']
---

It's done, finally merged. Hundreds of commits, 15 issues closed, a monster is dead.

The biggest thing I'm noticing right now is how slow the evening goes when you aren't crunching :D

I was looking at last week's post to see what I was up to and I realize that although I've been feeling like I was going really slow I've had 52 commits in the last week which includes the following:

### Added a simple api for user's to be able to inspect the compilation environment in their macros

Common Lisp allows you to access the environment in the macros, however the interface to interact with that object didnt make it into the standard. It was, however, documented so I can still read it [0]. I decided not to implement that api yet though and instead I just made the functions I felt made sense. It will be easy to revisit this when I feel the drive to.

### Added 'declare'

Lisp allows you to declare information about things in your code, whether it is whether a function should be inline or the type of a value, it's handled through `declare`

Once again, although it didn't make it into the standard, extending the kinds of things that can be declared has been thought about and documented. For now I just hooked it into my compile-time-metadata system. A little reading reveals that the proposed system is a rough superset of what I offer so at some point I will implement that and redefine by metadata using it.

### Added 'deftype'

This is one place I left the lisp spec entirely. `deftype` in lisp is mad, types are seen as sets of objects (makes sense) and they don't restrict you in how you define that. For example I can say:

```
;; a function that returns true if given an array where
;; all it's dimensions are of equal length
;;
(defun equidimensional (a)
   (or (< (array-rank a) 2)
       (apply #'= (array-dimensions a))))

;; the type of all square arrays
;;
(deftype square-matrix (&optional type size)
  `(and (array ,type (,size ,size))
        (satisfies equidimensional)))
```

This is super expressive but totally impossible to check at compile time as you can have arbitrary function calls in there. The compilers do a great job on using the flexibility in some cases though, which I won't yak about now.

In GLSL things are very different, we don't want to do anything at runtime unless we can help it. We can't make any types except structs and I already have a nice mechanism for that.

However there are so things that may be interesting.

- Making a 'special kind of' some exisiting type. For example a `rotation` type that is essentially a `vec3`. In the GLSL it will be a `vec3` however by making the type checker see it as a different type we could make it impossible to pass a position as the rotation argument to a `rotate` function by accident. I call these `shadow` types (I probably need to rethink this name :p).

- Making a type that only exists at compile-time for the purpose of holding metadata. This 'ephemeral' type never appears in the resulting GLSL and is an advanced feature but can be useful. I use these to represent vector spaces in my code and then use macros to inject code to create the matrices that transform between the spaces.

To make a shadow type I write `(deftype rotation () :vec3)`

To make an ephemeral type I write `(deftype space () ())`

In the case of shadow functions I also allow you to easily 'copy' functions that are defined for the type you are shadowing. This means you don't have to redefine addition, subtraction etc for your `rotation` type, you just copy it from `vec3`

### Mooore Types

One problem with how I used to write lisp is that too often I would use lists when I should have used classes. This has bitten me in the arse one to many times and so I refactored a bunch of stuff to have their own types.

This will help with future refactoring and also as I start working out the 'final' public api for my compiler.

### Rewrite Space Feature in CEPL

`Spaces` are a feature I have spoken of before but I will quickly recap. They are a fairly simple idea, you define a scope with a vector space and inside that scope all vectors have to be in that space. If you try and use a vector from another space it will be automatically converted to the correct space before anything else happens. This lowers the chances of messing up your vector math and makes the code more self-documenting.

Previously this was implemented by:

- compiling the code once
- analysing the AST for places that needed conversions
- patching the code with the needed conversions
- recompiling and using the new GLSL

This felt super hacky as all of the process was specific to my compiler. I have now rewritten it using macros, there is much less that will feel alien to a lisper[1] and it all happens in one pass now.

### Change the default minify-filter for samplers in CEPL

I thought I had a bug in the textures or samplers in CEPL. I was using the `textureLod` function in my shaders but no matter what mipmap level I chose nothing would change.

I spent hours looking at the code around mipmaps and finally found out that nothing was wrong, but I had misunderstood how things were meant to work. I had imagined that `textureLod` allowed you to query the colors from a texture's mipmaps regardless of the filter settings, whereas actually you need to `minify-filter` to either linear-mipmap-nearest or linear-mipmap-linear before ANY querying using `textureLod` is possible.

CEPL defaulted to just `linear` as it felt the simplest and I didnt want to confuse beginners (irony goes here) but instead I created a behaviour that was surprising and required knowledge to understand. This interests me and I'll muse over this in future when designing apis.

### Bugfixes EVERYWHERE

Just whole bunch of other stuff. Not much to note here other than how quickly trying to make nice errors seems to balloon code.

For example if you are processing 10 things, and one fails, it is easy to detect this in the `process_thing` function and throw an error. However this means that the user may fix the one thing retry and hit the same error on the next thing. Really we want to aggregate the errors and present an exception saying 'these 15 things failed for these reasons'. This would give the user more chance of seeing the patterns that are causing problems. This is especially true with syntax error in your code. You don't want them one at a time, you want them on aggregate in order to see what you are doing wrong.

This also means that your error messages should be able to adjust depending on the number of things that went wrong. For example when 15 things break the 'these 15 things failed for these reasons' message is cool, but if only 1 thing broke we want 'this thing failed for this reason'. These subtle language differences can make a huge difference to how the user feels about the tool they are using. Pluralizing words when there is only one thing sounds dumb and just adds to the annoyance of the tool not working.

I don't have ideas for a clean solution to this yet, but one day I want something for this.

### Sleep

I'm damn tired now. I'm off to bed.

Peace

[0] https://www.cs.cmu.edu/Groups/AI/html/cltl/clm/node102.html
[1] and once I have the stuff from [1] it will be even better
