---
layout: post
title: Varjo and Multiple Value Return
description:
date: 2014-07-24 15:46:00
category:
tags: ['lisp', 'cepl', 'varjo']
---

Hi folks, having recently got married (yay!) and got a flat (another yay!) I have been getting back into lisp coding again.

The latest addition to the Varjo compiler is support for 'values' and 'multiple-value-bind'. Now as glsl doesn't have support for multiple value returns we have to use something else. The mechanism I have now works something like this:

* If you have a 'values' form within a multiple-value-bind form the multiple-value-bind form turns into a 'let' and the 'values' becomes 'setf's to the variables in the let.

* If you have a values form and it isnt within a multiple-value-bind then one of the following happens
** If the values form can propagate to a 'return' form then the function is given out-vars and they are set from the place where the values form was originally.
** If it cant reach a return then it collapses to a progn.

If a varjo function with out-vars is used within a multiple-value-bind then the multiple-value-bind becomes a let statement and the first var is set by the function return and the rest are passed as out-vars to the function.

If the function is used outside of a multiple-value-bind then the form is surrounded in a 'let' as the outvars still need to be set.

This results is something like the following.


    VARJO> (defshader test ()
             (labels ((thing ((x :int)) (return (values x (* x 2)))))
               (multiple-value-bind (x y) (thing 1)
                 (+ x y)))))

    ;; Gets compiled to

    #version 330

    int thing_26v(int x_23v, out int return1);

    int thing_26v(int x_23v, out int return1) {
        return0 = x_23v;
        return1 = (x_23v * 2);
        return return0;
    }

    void main() {
        int a_27v0;
        int a_27v1;
        a_27v0 = thing_26v(1,a_27v1);
        (a_27v0 + a_27v1);
    }

I still have a few bugs to iron out but it is very nearly there.
One step closer to lisp!