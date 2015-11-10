---
layout: post
title: (setf mayhem damnit)
description:
date: 2015-11-10 20:54:27
category:
tags: ['lisp', 'cepl', 'varjo']
---

When I was making varjo I hacked in support for setf very quickly. It gave the rough feeling of [generalized references](http://www.lispworks.com/documentation/HyperSpec/Body/05_aa.htm) but even at the time I was thinking that this hack would come back to kick me in the arse..Turns out yesterday it took a run up first..

So I had to take a detour from my detour writing the flow analyzer to rewrite setf. Luckily that wasnt too bad and the code makes way more sense now. So tonight I'm getting back to the flow analyzer and looking at `if` expressions.

These are a bit fiddly as you (usually) cant tell ahead of time which result is going to be returned. So from the point of the flow analyzer we will say they both do. So if:

    (let ((a 2)
          (b 3))
      (if (something-something-stuff)
          a
          b))

Then we have to assume both `a` & `b` return and continue accordingly.

But wait, what if there are side effects in there?

    (let ((c 2))
      (if (something-something-stuff)
          (setf c 10)
          (setf c 20))
	  c)


Then we have to be aware that the setf is in a condition when we update the flow information.

It's probably something people who know this kind of thing think is trivial but the way I've implemented this so far wasnt done in a way that took this into account. Still exciting though and will be cool when Ι crack it.

Back with more soon
