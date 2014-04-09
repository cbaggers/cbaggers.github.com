---
layout: post
title: Cepl in Time
description:
date: 2014-04-04 18:23:46
category:
tags: ['lisp', 'cepl']
---

This post is about time and my ongoing experiments of trying to find a way of representing it in cepl. If you aren't interested in the journey but are interested in the solution then jump down to the 'So what's the result?' section.

###Why Bother?

It would be very valid to raise the fact that for real-time applications there are some very well-know and suitable ways of handling time (e.g. [fixed timestep](http://gafferongames.com/game-physics/fix-your-timestep/) ) so why wouldn't we use these techniques?

Often when coding interactively you just want to run something in the repl in pure isolation. These kind of experiments can give rise to new techniques and allow you to toy with idea more easily as you don't have to worry about any other part of your program. Lisp support this style of programming by having axiomatized some of the core concepts of the language[1] and I wanted that for time as well. I didn't want to have to take some time delta from another part of my program in order to use it in some function or concept I was developing, I want to focus purely on the task at hand.

###What should time look like?

So after I had decided I wanted some kind of primitive for time in cepl I needed to decide what this would look like. As usual I stole from lisp. Cons cells encapsulate the most basic nature of a list: It is some 'thing' followed by some other 'thing'. So what is time?

Well first off I decided not to look at it from a physics point of view, as  this would be unnecessarily complicated, and stick to how time seems to be. Well time is continuous, it doesn't seem to 'arrive' in lumps... but computers by their nature are incremental. So if time is flowing and we treat it like a stream of water, we want to capture time in buckets and then use the captured time to drive some function. OK, that's weird but time-buffers are kind of what fixed-time-steps implement so lets have 'time-buffers' as a concept.

Next...where does the time come from? Well in computing we are used to having streams of data coming from some data source so let's have a time-source. It also makes sense that we might want to speed up and slow down time so let's have a time-source be able to be based on another time source with a modifier. We could use this to make a time source that is running 10 times slower than real time. Now if we allow passing of time sources to our time buffers we have a side effect free way of implementing slow motion...even if this only temporary it could be useful for observing some effect slowly to see it is behaving correctly.

When we speak about time we often use some condition to qualify when we are talking about. For example:

* BEFORE tomorrow do x
* BETWEEN 10am and 2pm do x
* AFTER 10 minutes from now do x

These predicates should take a time an return t or nil.

The 'AFTER' example is interesting as it includes some syntax for talking about time relative to another point in time. So we will need some kind of 'from-now' function.

###The weird bit...

OK so this next part of the process was not reasoned about or based on logic. I was writing down the above and trying to work out other time predicates when I heard a voice in my head say ***'LEXICAL TIME'***. This was an odd thing to hear as I had no idea what my brain was talking about..so I stopped for a bit the more I thought about it the more I started to like the idea, we have a temporal-function so you define functionality that implicitly understood time.

I thought it could be some macro that wrapped up the elements we had already created and let the user write code knowing that time was being taken care of.

    ;;this was the first pseudo-code of a temporal-lambda
    (tlambda (x) (within 0 5000) (print x))


###The problems

####Expiry

The first problem I noticed was in a tlambda that uses the 'before' predicate e.g:

    (tlambda () ((before (from-now (seconds 10))) (print "hi")))

Notice that after 10 seconds from now this function will never run again. It essentially has expired. Now I could return some kind of value but this sucks as I was these temporal functions to be identical to regular functions. A friend then recommended using conditions, which turned out to be perfect. So we now have an 'expired' condition that can be caught by time aware code.

But when we look at this again we notice that really expired isnt limited to time...it is part of a more general set of  conditional-functions. Which crudely stated as a macro could be.

    (defmacro defcfun (name args condition &body body)
      `(defun ,name args
         (if ,condition
             (progn ,@body)
             (signal-expired))))

So temporal functions are a type of conditional function. I'm not sure how useful this is...but it was interesting to me.

####Composition and Syntax

The next big issue was in composition and what the resulting code looked like. I was starting to get something working with functions and closures but I started running into a problem as I wanted a predicate that returned t once every x seconds/milliseconds.. something like:

    (tlambda () ((each (second 2)) (print "hi"))

But what we actually need is to create a closure with a stepper in the closed vars.

    (let ((stepper (make-stepper (seconds 2))))
       (tlambda () (when (funcall stepper) (print "Hi")))

Given that tlambdas were meant to implicitly understand time this seemed ugly.

This was compounded by the next problem...

####Time overflow

Let us define an imaginary tlambda with a nice syntax:

    (tlambda ()
      (then ((before (from-now (seconds 5))) (print "hi 1"))
            ((before (from-now (seconds 5))) (print "hi 2"))
            ((before (from-now (seconds 5))) (print "hi 3"))))

What this is meant to do is:

* run the first before clause until it expires and then
* run the next clause until expires and then
* run the third clause until it expires and the signal that the whole tlambda has expired.

Now let's imagine that we create this tlambda but wait 12 seconds before calling it. The first time we call it should now recognize that the first two clauses have already expired and jump straight to 2 seconds into the third clause.

At this point I could not think of a way of doing this that didnt invlove a DSL. This became even more apparent when I thought about optimizing as I can't inline the stepper closures and inside the tlambda there would be A LOT of signals being fired and caught.

I was quite annoyed that my knowledge of lisp was potentially limiting me here but at least I could have a go at making a macro version which could be more optimized (if perhaps not as functional as I would originally have liked)

####So what's the result?

Well the result is a very nice DSL which creates lambdas that know time and can mix temporal syntax with default lisp syntax very easily.

Here are a few examples:

    ;; when you use default lisp code, the result is a regualr lambda
    (tlambda () (print "Hi"))

    ;; inline lambdas are fine too, again this is just a regular
    ;; lambda
    (tlambda ()
      (let ((text "hello"))
        ((lambda (x) (print x)) text)))

    ;; This function will for the first 10 seconds print "hi jim" each
    ;; time it is called. After 10 seconds it will print "jim" each
    ;; time it is called.
    ;; Temporal forms are conditional, the predicate must be true for
    ;; the rest of the form to be evaluated. For this reason we borrow
    ;; the syntax from the cond macro.
    ;; Also notice that unlike cond every line is evaluated. So the
    ;; (print "jim") line always works. This also means this function
    ;; never actually expires. regular lisp forms can be seen as
    ;; eternal.
    (tlambda ()
      ((before (from-now (seconds 10))) (princ "hi "))
      (print "jim"))

    ;; time syntax must be either at the root of the tlambda or inside
    ;; one of the special forms defined for time functions.
    ;; the following is not valid, as the temporal condition form is
    ;; within a standard lisp form
    (tlambda ()
      (print ((before (from-now (seconds 10))) "hi")))

    ;; tlambda introduces 2 new special forms: then & repeat
    ;; they each run each containing form until it expires and
    ;; only then move to the next form. The difference is that
    ;; once all the forms have expired 'then' will signal it has
    ;; expired, whereas repeat will start from the first form again
    (tlambda () (then ((before (from-now (seconds 10))) (print "hi"))
                      ((once) (print "allo")))

    (tlambda () (repeat ((before (from-now (seconds 10))) (print "hi"))
                        ((once) (print "allo")))

    ;; progn is also modified so that it can contain the temporal
    ;; condition forms
    (tlambda ()
       (progn ((before (from-now (seconds 2))) (print "hi"))))

####Expansion

Here is an example of what is generated by tlambda (I have tidied it up a little and changed gensym names for ease of reading but functionally it is the same):

    ;; This:
    (tlambda ()
      (then ((before (from-now (seconds 3))) (print "hi"))
            ((before (from-now (seconds 4))) (print "bye"))))

    ;; expands too:
    (let ((counter 0)
          (deadline-1 (from-now (seconds 3)))
          (deadline-2 (from-now (seconds 4))))
      (lambda ()
        (if (= counter 2)
            (signal-expired)
            (when (and (< counter 2))
              (if (afterp deadline-1)
                  (progn
                    (when (= counter 0)
                      (incf counter)
                      (setf deadline-2 (- (from-now (seconds 4))
                                          (- (funcall *default-time-source*)
                                             (afterp deadline-1)))))
                    (if (afterp deadline-2)
                        (when (= counter 1) (incf counter))
                        (when (beforep deadline-2) (print "bye"))))
                  (when (beforep deadline-1)
                    (print "hi")))))))

####What is missing?

* Tests and Bugfixing: There are still bugs in this and need to squash them
* Generate cleaner code: Currently it works well but could be nicer for human reading
* Generate type declarations: For speed optimization

Well that is all for now. Thanks for reading. The interesting thing now will be playing with this and seeing if the abstraction is actually useful!

Ciao



*1.* The simplest example of this is lists in lisp. Rather than just being given a list type, with it's implementation hidden away, we get the cons cell. The cons cell is the smallest abstraction of a linked list and with it we can build lists, tree and other graphs and then use the techniques we have for navigating cons cells (car and cdr (and friends)) to managed all of these different data structures. Also see [Paul Graham's 'The Roots of Lisp'](http://lib.store.yahoo.net/lib/paulgraham/jmc.ps). It's a short article, but it hows how with 7 basic primitives you can write an interpreter for an entire lisp language.
