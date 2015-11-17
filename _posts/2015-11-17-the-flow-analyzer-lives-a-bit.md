---
layout: post
title: The flow analyzer LIVES (a bit)
description:
date: 2015-11-17 01:38:55
category:
tags: ['lisp', 'cepl', 'varjo', 'nanowrimo']
---

Had a couple of days off coding but now I'm back. Yet again Ι found limits in the my compiler's architecture, this time in how I handled environments.

So before what Ι had was roughly as follows:

I have a tree of code that needs compiling. The compiler walks down the tree and compiles each form. The environment is simply an object that holds all the variables and functions that exist at that point in the program. To implement variables scope I simply copy the environment as it was at the start of the scope, add the new variable/s and then throw it away at the end. Because the stuff outside the scope only see's the original environment it couldnt access variables from the inner scope.

This has worked great for ages but now by flow analyser wants the answer to the question "what scope was this variable made in?" and that data has been thrown away.

So now I've switched to a tree of environments, and they are now all immutable. This was a damn big change so Ι took the opportunity to start using a testing framework so that I could build basic tests while getting everything working again. This went really well so now Ι can show the following.


Let's take this code. It's useless but you can see that we make two vectors (that's the `v!` forms) each starting with x

    (defshader test ()
      (let ((x 1))
        (v! x 0)
        (progn
          (let ((z 3))
            x))
        (v! x 0 0 0)))

Now, if I let the flow analzer do it's work and inspect the data for the `v!` calls, I get something like.

    (V! 514 515)
    (V! 514 518 519 520)

The numbers are called `flow-id`s, they are used to track the flow of a value through a program. If two are the same then they are the same value (even if they get bound to different variables or passed through functions).

So above we can see that `514` appears as the id of the first arg for both `v!` forms, that means it's not just the same variable (`x`) but the same value too.

Now in the following example we set `x` to be a different value (it's in a progn and a let so I could see if it would track the flow of executiong properly).

    (defshader test ()
      (let ((x 1))
        (v! x 0)
        (progn
          (let ((z 3))
            (setq x z)))
        (v! x 0 0 0)))

The flow analysis of the `v!` calls in this program look like this

    (V! 506 507)
    (V! 509 510 511 522)

This time the `flow-id`s of the first argument are different. This shows that, even though they both take `x`, the system knows the value has been changed.

With these fixes Ι can finally handle conditions as the trees of environements are trivial to diff, so Ι will be able to combine the flow-ids from the two execution paths.

Thanks for reading!
