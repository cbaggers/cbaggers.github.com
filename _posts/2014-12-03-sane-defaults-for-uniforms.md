---
layout: post
title: Sane defaults for Uniforms
description:
date: 2014-12-03 08:24:49
category:
tags: ['lisp', 'cepl']
---
OK so at the end of the last post I started musing about having sensible defaults for uniforms values. The logic behind this follows:

So we have a shader 'vert'

    (defvshader vert ((position :vec4) &uniform (i :int) (loop :float))
      (let ((pos (v! (* (s~ position :xyz) 0.3) 1.0)))
        (setf gl-position (+ pos (calc-offset (+ (float i)) loop)) )))

And now I want to add a new uniform: (thing :int)

So do I add the new argument to vert, compile and it freaks out as it has no value? Or do I add it to the calls to vert first and watch it freak out as the shader doesn't take this?

Both modes suck, and it obviously makes sense to modify the shader first, but on first compile have some default value that won't cause the shader to panic.

This is easy for numbers, vectors and matrices as all have a concept of an identity, but what about structs?

Well all gl-structs ultimately have to be composed of types glsl understands, therefore we could have the defglstruct method generate an identity value we can use.

What about textures? Do we create a default unit texture for each sampler type? Could do. And we don't have to worry about 'index out of bounds' as by default indexes just wrap for textures. (this is not true for texelFetch, the result is undefined, but shouldnt crash so will but enough time to provide real values)

according to here: http://stackoverflow.com/questions/6354208/glgentextures-is-there-a-limit-to-the-number-of-textures

    "The only limit of glGenTextures is given by the bit width of the texture name (GLint), which is 32 bit;"

Which means one (1x1..n) texture for each of the 18 sampler types shouldn't be an issue.

Now what about uniform buffers? I guess I will want something similar to gl-structs, but as I haven't implemented UBOs yet I won't worry about it until I am.

Right, back soon!

Ciao