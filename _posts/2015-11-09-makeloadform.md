---
layout: post
title: make-load-form
description:
date: 2015-11-09 00:06:19
category:
tags: ['lisp']
---

This post is more of reminder to myself than anything else.
Just now Ι made a little macro that not only emitted regular code (symbols and lists) but also had an instance of a class in there. This caused an error Ι hadnt seen before:

    ; caught ERROR:
    ;   don't know how to dump #<FLOW-IDENTIFIER :ids (-2905)> (default MAKE-LOAD-FORM method called).

A quick google found [this](http://coding.derkeiler.com/Archive/Lisp/comp.lang.lisp/2011-03/msg00539.html) this the excellent answer by Mario:

	The problem you are having is due to the fact your the macro not only
	creates code, but that this code has actual live objects embedded into
	them in this case. That's the def-collection thing. That is not
	necessarily a problem, but in this case it is because sbcl does not know
	how to save such an object in a fasl. You can either figure out how to
	make an appropriate load form, or do something that avoids the use of
	the actual live object.

Intriguing, so 'load forms' but be a concept in lisp. Let's have a look:

[CLHS: make-load-form](http://clhs.lisp.se/Body/f_mk_ld_.htm#make-load-form)

Cool, so it seems that the mechanism for definign how literals are serialized and deserialized by the compiler (sorry if those terms are inaccurate for this lisp feature) and it also seems this can apply to structures and standard objects if you implement make-load-form:

	structure, standard-object: Objects of type structure-object and
	standard-object may appear in compiled constants if there is an
	appropriate make-load-form method defined for that type.

	The file compiler calls make-load-form on any object that is
	referenced as a literal object if the object is a generalized instance
	of standard-object, structure-object, condition, or any of a (possibly
	empty) implementation-dependent set of other classes. The file
	compiler only calls make-load-form once for any given object within a
	single file.

I still feel amazed at how well common lisp turned out, I get lovely suprises like this all the time as I get deeper into the language.

Anyway I need to read deeper into this, looks like fun
