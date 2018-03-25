---
layout: post
title: Lisping Furiously
description:
date: 2018-03-25 19:23:09
category:
tags: ['lisp', 'varjo', 'cepl']
---

This week has been pretty productive.

It started with looking into a user issue where compile times were revoltingly slow. It turns out they were generated very deeply nested chains of `if`s and my code that was handling indenting was outstandingly bad, almost textbook example of how to make slow code. Anyway after getting that fixed up I spent a while scraping back milliseconds here and there.. there's a lot of bad code in that compiler.

Speaking of that, I knock Varjo a bunch, and I'd never make it like this again, but as a vessel for learning it's been amazing, most issues I hear about in compilers now I can at least hook somewhere in my head. It's always 'oh so *that's* how real people do this', very cool stuff. Also, like php, varjo is still unreasonably still providing me with piles of value; what it does is still what I wanted something to do.

After that I was looking into user extensible sequences and ended up hitting a wall in implementing something like [extensible-sequences](http://www.doc.gold.ac.uk/~mas01cr/papers/ilc2007/sequences-20070301.pdf) from sbcl. I really want `map` & `reduce` in Vari, but in static languages this seems to mean some kind of iterator.

It would really help to have something like interfaces but I can't stand the idea of not being able to say how another user's type satisfies the interface, so I started looking into adding traits :)

I was able to hack in the basics and implement a very shoddy `map` and could get stuff like this:

    TESTS> (glsl-code
            (compile-vert () :410 nil
              (let* ((a (vector 1.0 2.0 3.0 4.0))
                     (b (mapseq #'(sin :float) a)))
                (vec4 (aref b 0)))))

    "// vertex-stage
    #version 410

    void main()
    {
        float[4] A = float[4](1.0f, 2.0f, 3.0f, 4.0f);
        float[4] RESULT = float[4](0.0f, 0.0f, 0.0f, 0.0f);
        int LIMIT = A.length();
        for (int STATE = 0; (STATE < LIMIT); STATE = (STATE++))
        {
            float ELEM = A[STATE];
            RESULT[STATE] = sin(ELEM);
        }
        float[4] B = RESULT;
        vec4 g_GEXPR0_767 = vec4(B[0]);
        gl_Position = g_GEXPR0_767;
        return;
    }

but the way array types are handled in Varjo right now is pretty hard-coded when I had it making separate functions for the `for` loop each function was specific to the size of the array. So 1 function for `int[4]`, 1 function for `int[100]` etc.. not great.

So I put that down for a little bit and, after working on a bunch of issues resulting in unnecessary (but valid) code in the GLSL, I started looking at documentation.

This one is hard. People want docs, but they also don't want the api to break. However the project is beta and documenting things reveals bugs & mistakes. So then you have to either document something you hate and know you will change tomorrow, or change it and document once.. For a project that is purely being kept alive by my own level of satisfaction it has to be the second; so I got coding again.

Luckily the generation of the reference docs was made much easier due to [Staple]() which has a fantastically extensible api. The fact I was able to hack it into doing what I wanted (with some albeit truly dreadful code) was a dream. 

So now we have these:

- [Reference Docs for the Vari language](http://techsnuffle.com/varjo/vari-reference.html)
- [Reference Docs for the Varjo compiler](http://techsnuffle.com/varjo/varjo-reference.html)
- [The start of a user guide for Varjo](https://github.com/cbaggers/varjo/blob/master/docs/user-guide.md)

Bloody exhausting.

The Vari docs are a mix of GLSL wiki text with Vari overload information & handwritten doc strings for the stuff from the Common Lisp [portion of the api](http://techsnuffle.com/varjo/vari-reference.html#COMMON-LISP).


That's all for now. On holiday for a week and I'm going to try get as much lisp stuff out of my head as possible. I want my lisp projects to be in a place where I'm focusing on fixes & enhancements, rather than features for when I start my new job.

Peace
