---
layout: post
title: Trials and Tessellations
description:
date: 2017-05-02 08:42:17
category:
tags: ['lisp', 'cepl', 'varjo']
commentIssueId:
---

Tessellation works! I was [able to replicate](https://github.com/cbaggers/cepl.examples/blob/master/examples/tessellation.lisp#L37) the fantastic tutorial from [The Little Grasshopper](http://prideout.net/blog/?p=48) and finally get a test of a pipeline with all 5 programmable stages.

This took a little longer than expected as my GPU complained on the following shader

> Error compiling tess-control-shader:
> 0(6) : error C5227: Storage block _FROM_VERTEX_STAGE_ requires an instance for this profile
> 0(13) : error C5227: Storage block _FROM_TESSELLATION_CONTROL_STAGE_ requires an instance for this profile

```
#version 450

in _FROM_VERTEX_STAGE_
{
     in vec3[3] _VERTEX_STAGE_OUT_1;
};

layout (vertices = 3) out;

out _FROM_TESSELLATION_CONTROL_STAGE_
{
     out vec3[] _TESSELLATION_CONTROL_STAGE_OUT_0;
};

void main() {
    vec3 g_G1327 = _VERTEX_STAGE_OUT_1[gl_InvocationID];
    vec3 g_GEXPR01328 = g_G1327;
    _TESSELLATION_CONTROL_STAGE_OUT_0[gl_InvocationID] = g_GEXPR01328;
    float TESS_LEVEL_INNER = 5.0f;
    float TESS_LEVEL_OUTER = 5.0f;
    if ((gl_InvocationID == 0))
    {
        gl_TessLevelInner[0] = TESS_LEVEL_INNER;
        gl_TessLevelOuter[0] = TESS_LEVEL_OUTER;
        gl_TessLevelOuter[1] = TESS_LEVEL_OUTER;
        gl_TessLevelOuter[2] = TESS_LEVEL_OUTER;
    }
}
```

As far as I can tell, the above code is valid according to the spec, but my driver wasn't having any of it[0]. This meant I had to do some moderate code re-jiggling. You can't just add an `instance` name to the blocks as then you get:

> Error Linking Program
> 0(13) : error C7544: OpenGL requires tessellation control outputs to be arrays

Grrr. This makes total sense according to the spec though, so grr goes back to error `C5227`. FUCK YOU `C5227`.

Anyway that got fixed and this happened

![yay](/assets/images/tess.png)

With that done I made sure the same worked using CEPL's [inline glsl stages](https://github.com/cbaggers/cepl.examples/blob/master/examples/tessellation-inline-glsl.lisp#L38) and tested a bunch.

This left me at an odd point. CEPL has a lot less gaps in the API that it used to[1], it feels kind of ready for me to use.

So I sat down with [The Book of Shaders](https://thebookofshaders.com/) and got started. I simply can't give that project enough praise, it's wonderfully written and remains interesting whilst also pushing you just enough to really digest the details of what they want you to learn. I made a little [shadertoy](https://www.shadertoy.com/) substitute[2] and started doing the exercises. I have to say I'm pretty proud with how fun it all felt, it's been a lot of work to get something that is lispy but doesn't feel hampered by being the non-native language, so those hours of play were very vindicating.

Within a very short time [The Book of Shaders](https://thebookofshaders.com/) is getting you to amass a collection of helper functions for your shaders. I already have a (very wip) project called [Nineveh](https://github.com/cbaggers/nineveh) for this purpose, so it's time to start working on that again! I want a kind of 'standard library' for CEPL, somewhere you can find a pile of implementations of common functions (e.g. noise, color conversion, etc) to either use to at least use as a starting point.

There are often cases where libraries like this fall short as you want a slight variation on a shader. Say for example you like their [FBM](http://www.iquilezles.org/www/articles/warp/warp.htm) function but want one more octave of noise. I'm hoping macros can help a little here as we can provide something that generates the variant you need.

This is likely naive but it'll be fun to try.

Right, that's my lot for now. Seeya!

-------------------

[0] I was meant to be using the compatibility profile too so I'm not sure what was up

[1] See [last weeks post](http://techsnuffle.com/2017/04/26/checking-in) for what remains

[2] There's about 50 lines not shown here which handles input & screen events.
```
(in-package :ctoy)

(defun-g ctoy-quad-vert ((pos :vec2))
  (values (v! pos 0 1)
          (* (+ pos (v2! 1f0)) 0.5)))



(defun-g ctoy-quad-frag ((uv :vec2)
                         &uniform (now :float)
                         (mouse :vec2) (mouse-norm :vec2) (mouse-buttons :vec2)
                         (screen-res :vec2))
  (v! (/ (s~ gl-frag-coord :xy) screen-res) 0 1))



(def-g-> draw-ctoy ()
  :vertex (ctoy-quad-vert :vec2)
  :fragment (ctoy-quad-frag :vec2))

(defun step-ctoy ()
  (as-frame
    (map-g #'draw-ctoy (get-quad-stream-v2)
           :now (* (now) 0.001)
           :mouse (get-mouse)
           :mouse-norm (get-mouse-norm)
           :mouse-buttons (get-mouse-buttons)
           :screen-res (viewport-resolution (current-viewport)))))

(def-simple-main-loop ctoy
  (step-ctoy))
```
