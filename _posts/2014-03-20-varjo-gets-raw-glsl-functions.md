---
layout: post
title: Varjo Gets Raw Glsl Functions
description:
date: 2014-03-20 20:11:31
category:
tags: ['lisp', 'varjo']
---

Yesteday I added the ability for varjo to use blocks of raw glsl code which was a feature I have had a couple of requests for.

The raw blocks are presented as functions and you have to specify the arguments and return type using varjo syntax. This allows varjo to perform the type checks that make this raw function safe to use in your shaders.

The body however is a string and as such I am not doing any checks to ensure the code is valid. If you lie to the compiler things will break :)

This is how a function can be created and used in shaders. Notice that like all external functions it will be automatically included if used in any shader.

    VARJO> (v-def-raw-glsl-func raw-test ((a v-int) (b v-float)) v-float
             "return a * b;")
    T

    VARJO> (defshader test-shader ()
             (raw-test 1 2.4))
    #<VARJO-COMPILE-RESULT {25EA8929}>

    VARJO> (glsl-code *)
    "#version 330

    float raw_test_1v(int a, float b);

    float raw_test_1v(int a, float b) {
    return a * b;
    }

    void main() {
    raw_test_1v(1,2.4f);
    }
    "

One thing I would also like to add is some extra syntax to allow calling out to shader functions written in lisp. This should be pretty easy but I have a HUGE list of other things to implement first so that will have to wait for now.

Seeya folks
