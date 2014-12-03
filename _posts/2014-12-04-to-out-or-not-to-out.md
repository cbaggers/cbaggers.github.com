---
layout: post
title: To out or not to out
description:
date: 2014-12-03 23:49:36
category:
tags: ['lisp', 'cepl']
---

    (defpipeline prog-1 ((vert vert-data))
      (:vertex (setf gl-position (pos vert))
               (out (the-color :smooth) (col vert)))
      (:fragment (let ((lerp-value (/ (y gl-frag-coord) 500.0)))
                   (out outputColor (mix the-color 
                                         (v! 0.2 0.2 0.2 1.0)
                                         lerp-value)))))

Currently setting the out vars of a shader stage is done with the out special form. The above code compiles to

    // vertex shader
    #version 330

    layout(location = 0) in vec4 fk_vert_position;
    layout(location = 1) in vec4 fk_vert_colour;
    smooth out vec4 the_color;

    void main() {
        gl_Position = fk_vert_position;
        the_color = fk_vert_colour;
    }

    // fragment shader
    #version 330

    smooth in vec4 the_color;
    out vec4 outputcolor;

    void main() {
        float lerp_value_4v = (gl_FragCoord.y / 500.0f);
        outputcolor = mix(the_color,vec4(0.2f,0.2f,0.2f,1.0f),lerp_value_4v);
    }

Notice how the out form essentially compiles to a global var and a setf.

In the quest for lispyness, should this stay as it is? There isn't anything wrong with setf per se, however it is worth investigating other forms to see if anything surprises us...and by us I mean me :)

Here is a version with a version of values that allows naming

    (defpipeline prog-1 ((vert vert-data))
      (:vertex (values (gl-position (pos vert))
                       ((the-color :smooth) (col vert))))
      (:fragment
       (values (outputColor
                (let ((lerp-value (/ (y gl-frag-coord) 500.0)))
                  (mix the-color 
                       (v! 0.2 0.2 0.2 1.0)
                       lerp-value))))))

Eh..the vertex shader is ok but this introduces an extra level of indentation that looks ugly

Ok so how about setf style? Setf allows a name-value style to set multiple variables

    (setf a 1
          b 2)

So for the shaders that is...

    (defpipeline prog-1 ((vert vert-data))
      (:vertex (values gl-position (pos vert)
                       (the-color :smooth) (col vert)))
      (:fragment
       (values
        outputColor
        (let ((lerp-value (/ (y gl-frag-coord) 500.0)))
          (mix the-color 
               (v! 0.2 0.2 0.2 1.0)
               lerp-value)))))

.. nope, doesn't feel better aesthetically and now makes identifying the val and name forms more difficult.

Declare style?

    (defpipeline prog-1 ((vert vert-data))
      (:vertex
       (declare (out gl-position (the-color :smooth)))
       (values (pos vert) (col vert)))
      (:fragment
       (declare (out outputColor))
       (values (let ((lerp-value (/ (y gl-frag-coord) 500.0)))
                 (mix the-color 
                      (v! 0.2 0.2 0.2 1.0)
                      lerp-value)))))

..Interesting, yesterday I was looking at declare for types, so if that was the case then using this would feel natural.

Lets see what happens if the types are in declare too

(defpipeline prog-1 (vert)
  (:vertex
   (declare (vert-data vert) (out gl-position (the-color :smooth)))
   (values (pos vert) (col vert)))
  (:fragment
   (declare (out outputColor))
   (values (let ((lerp-value (/ (y gl-frag-coord) 500.0)))
             (mix the-color 
                  (v! 0.2 0.2 0.2 1.0)
                  lerp-value)))))

Haha, not much as almost everything here was already being done with inference.

There is still an issue though that is best summed up in Erik Naggum's quote on c++. For my purposes the fact that this is about c++ is irrelevant.

    C++ is philosophically and cognitively unsound as it forces a violation
    of all known epistemological processes on the programmer.  as a language,
    it requires you to specify in great detail what you do not know in order
    to obtain the experience necessary to learn it. -- Erik Naggum

Declaring types in advanced forces the programmer to make an assertion about the nature of the code not yet written.
This violates the concept of live coding as we are coding, not because we have the solution, but because we are looking for one.

Now obviously with coding in the restricted environment of the GPU types are currently a necessity so the best we can do is minimize the cognitive damage. Other than having inference everywhere we possibly can, the other way is to delay specifying the type until it is actually needed, in most cases with the arg definition.

Hmm so no progress on this but I'm glad I've been able to empty my head a little.

Goodnight