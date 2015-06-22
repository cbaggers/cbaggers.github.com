---
layout: post
title: Machine Readable GLSL Spec
description:
date: 2015-06-22 08:27:04
category:
tags: ['lisp', 'cepl']
---

TLDR: [Look Here](https://github.com/cbaggers/gl-to-sexp/blob/master/spec/glsl)

There was one thing during making my glsl compiler that was terrible; One thing worse than any of the bugs I have hammered out so far.

The GLSL Spec

The spec is available either as pdf or html. It is designed for humans to read but I needed my program to know about every function in glsl. To that end I spent days processing the contents of the pdf, and some other open code, in order to get a list of all the functions with their return & argument types.

Since then I have wanted to update all the functions with information on which versions they are available from, but this has seemed a daunting undertaking.

Just the other day however, some people have put together a new gl documentation site [http://docs.gl](http://docs.gl), whilst the information itself still is geared to be human readable it is at least better laid out and stored in a github repo.

I took a few hours over the weekend to knock up a script that would extract the basic info I need from the glsl spec. Potentially I could do the opengl one too, but for now that is not urgent. You can find the results [here](https://github.com/cbaggers/gl-to-sexp/blob/master/spec/glsl).

The data is one big s-expression which contains all the glsl functions and variables.

#Layout

each element looks something like this

    (((("EmitStreamVertex" "void") (("stream" "int"))))
      (("EmitStreamVertex" NIL NIL NIL NIL NIL NIL :400 :410 :420 :430
      :440 :450)))

Each item has two elements:

- A list of function definitions
- A list of information on which version are supported

### Function definition
Each function definitions is laid out as follows:

- The first element is a pair of the function name as a string and the return type as a string.
- The rest of the list are pairs of the argument name as a string and the argument type as a string

### Version Information
Each element in the version info list is laid out as follows:

- The first element is the name of the function and in some cases enough arg info to differentiate it from other incarnations of the same function. This is one string which sucks, so I need to parse this so we can apply the version info directly.
- The rest of the elements are the versions supported. It is either `nil` or a keyword specifying the version. The `nil`s are becuase the orginal tables were rows of ticks of dashes to indicate versions supported. I turned the ticks into the version keyword and the dashes into `nil`. Really I need to remove the `nil`s and I will do that very soon.

## GLSL Variables
In the event that the function definition list is `nil` then the entry is a variable. You will find it's name as the first element in the version table along with the versions that support it.

Right I hope this helps. This will be in a state of flux for a while as I also want to extract all documentation for each entry in glsl and I'm sure there will be tweaks to be made.

I Hope it helps someone else.
