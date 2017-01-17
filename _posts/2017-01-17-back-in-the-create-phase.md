---
layout: post
title: Back in the create phase
description:
date: 2017-01-17 16:47:48
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

The other week I was nattering about how I feel I oscillate slowly between needing to create and needed to consume, these last 2 weeks I have definitely been back in the 'create' mode. I'm hammering away at the compiler and the new stuff is starting to feel nice. I won't bother with deep explanations as it's wouldn't work in this format, however I have been doing the following:

- Refactored up how the compiler compiles functions. I certainly can't say this code is clean yet, but it is at least not lazy. I used to just splice the function code in as a local function and let the code de-duplicator take care of things, this worked well enough for a while but I've been fighting it recently so I'm happy to sort this out.

- Tests! These have been worth their weight in gold this week and I've been tidying them up and adding a few more as I debug things.

- Moved the flow-id to the type: In short I have an ID that is used to work out where values (especially arguments) travel inside functions. When talking to a chap at work about this he mentioned 'dependent types' and this stuck with me. Given that this is metadata about the things flowing around, it is the kind of info that could live on a type. I have thus moved this to a field inside the type object and in doing so cleaned up a TONNE of things. This felt great.

- Started cleaning some of the code used to represent functions from the glsl spec and realized a bunch of it was redundant. Deleting code is heavenly.

- Deleted more old code related to types (from when I tried to encode the glsl spec's stupid gentypes directly )

- fixed assorted bugs

- found more bugs

- Started work on supporting the `void` type. Funnily enough I have not supported `void` so far in my compiler (except for the main function). The reason was that all code in lisp is an expression, so everything has a return (even if it's `nil`) to that end I worked to make sure this felt right in Varjo. However this has always left a few places that felt janky so I'm making void work now. This is pretty easy.

I'm basically going to be hammering on more of the same this week. I want to get this working and into master asap. One downside of the above is that in CEPL I had to introduce a hack to keep my `spaces` feature working. However I realized the other day how to replace all the crazy AST transforms I do in CEPL with a much simpler mechanism that is more general and will live in Varjo. More on this soon.

That's the lot.

Seeya
