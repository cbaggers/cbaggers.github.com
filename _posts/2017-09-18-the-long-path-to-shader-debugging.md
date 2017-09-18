---
layout: post
title: The long path to shader debugging
description:
date: 2017-09-18 09:32:42
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Writing shaders (in lisp or otherwise) is fun, however debugging them is not. Where on the CPU we get exceptions or error codes, on the gpu we get silence and undefined behavior. I really felt this when trying (and failing) to implement procedural terrain generation on the livestream. I tried to add additional outputs so that I could inspect the values but it was very easy to make a mistake and change the behavior of the shader..or worse to forget it was there and waste time debugging a side effect from the instrumentation. I need a more reliable way to get values back to the CPU. Luckily CEPL has some great places we can hide this logic.

Quick recap, in CEPL we define GPU functions and then compose them into a pipeline using `defpipeline-g`:

```
(defpipeline-g some-pipeline ()
  (vertex-stage :vec4)
  (fragment-stage :vec2))
```

This is a macro that generates a function called `some-pipeline` that does all the wrangler to make the gl draw call. You then use it by using `map-g`

```
(map-g #'some-pipeline vertex-data)
```

This is another macro that expands into some plumbing and (ultimately) a call to the `some-pipeline` function.

Putting aside other details what we have here is 2 places we can inject code, one in the function body and one at the function call-site. This gives us tonnes of leverage.

My goal is to take some gpu-function like this:

```
(defun-g qkern ((tc :vec2) &uniform (tex :sampler-2d) (offset :vec2))
  (+ (* (texture tex (- tc offset)) 0.3125)
     (* (texture tex tc) 0.375)
     (* (texture tex (+ tc offset)) 0.3125)))
```

And add calls to some function we will call `peek`.

```
(defun-g qkern ((tc :vec2) &uniform (tex :sampler-2d) (offset :vec2))
  (+ (peek (* (texture tex (peek (- tc offset))) 0.3125))
     (* (texture tex tc) 0.375)
     (* (texture tex (+ tc offset)) 0.3125)))
```

Peek will capture the value at that point and make it available for inspection from the CPU side of your program.

The way we can do it is to:

- compile the shader normally (we need to do this anyway)
- inspect the AST for calls to peek and the types of the argument
- create a new version of the shader with peek replaced with the instrumenting code

For example:

```
(defun-g qkern ((tc :vec2) &uniform (tex :sampler-2d) (offset :vec2))
  (let (((dbg-0 :vec2))
        ((dbg-1 :vec4)))
    (+ (setf dbg-1 (* (texture tex (setf dbg-0 (- tc offset))) 0.3125))
       (* (texture tex tc) 0.375)
       (* (texture tex (+ tc offset)) 0.3125))
    (values dbg-0 dbg-1)))
```

This code will work mostly the same way except that it will be returning the captured values instead of the original one. I say 'mostly' as now the code that doesnt contribute to the captured values is essentially dead code and it is likely that the GLSL compiler will strip chunks of it.

So now we have an augmented shader stage as well as the original, `defpipeline-g` can generate, compile and store these and on each `map-g` it can make 2 draw calls. First the debug one capturing the results using [transform-feedback](https://www.khronos.org/opengl/wiki/Transform_Feedback) (for the vertex stages) and FBOs for the fragment stage. Because `map-g` is also a macro we use it to implicitly pass the thread-local 'CEPL Context' object to the pipeline function. This lets us write debug values into a 'scratch' buffer stored on the context making the whole process transparent.

With this data available we can then come up with nice ways to visualize it. Just dumping it to the REPL will usually be a bad move as a single `peek` in a fragment shader is going to result in a value for every fragment, which (at best) means 2073600 values for a 1920x1080 render target.

There are a lot of details to work out to get this feature to work well[0], however it could be a real boost in getting real data[1] back from these pipelines and can work on all GL versions CEPL supports.

Seeya next week,
Peace.

`[0]:` transform feedback only works from the last implemented vertex stage, so if you have vertex, tessellation & geom stages, only geom can write to the transform feedback buffer.
`[1]:` Another option was to compile the lisp like shader language to regular lisp. However implementing the GLSL standard library exactly is hard and it's impossible to capture all the gpu/manufacturer specific quirks.
