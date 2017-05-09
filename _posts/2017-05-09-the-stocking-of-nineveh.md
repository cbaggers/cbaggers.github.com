---
layout: post
title: The stocking of Nineveh
description:
date: 2017-05-09 09:17:59
category:
tags: ['lisp', 'cepl', 'varjo', 'nineveh']
commentIssueId:
---

What a week. I'm a bit exhausted right now as I've been pushing a bit, but the results are fairly pleasing to me.

### GLSL Quality

First off I did a bunch of work in making the generated GLSL a bit nice to read. The most obvious issue was that, when I returned multiple values I would created a bunch of temporary variables. The code was all technically correct but it didn't read well. So now like this:

```
(defun-g foo ((x :float))
  (let ((sq (* x x)))
	(values (v2! sq)
			(v3! (* sq x)))))
```
can compile to glsl like this

```
vec2 FOO(float X, out vec3 return_1)
{
    float SQ = (X * X);
    vec2 g_G1075 = vec2(SQ);
    return_1 = vec3((SQ * X));
    return g_G1075;
}
```
Which is fairly readable. 1 temporary variable is still used, but that is to maintain the order of evaluation that was expected.

`return` is one of areas of the compiler with the most magic as we generate different code based on the context the code is being compiled in. It could be a return from a regular function (like above), or it could be from a stage in which case we need to turn it into `out` interface block assignments etc. This meant it took a bunch of testing to get it correct.

### Compiler cleanup

For a while I've been trying to clean up the code around how multiple return values are handled. I had a few false starts here (which ended in force-pushing away a day's worth of work) but it's done now. They are still plenty of scars in the codebase but the healing can at least begin :p

### Bugs in release

I then found a bug & a regression in the code I had prepped for the next release (into the common lisp package manager). As the releases are not made on a fixed cadence I wasn't sure how long I had so I dived into that. It turned out one of those cases where even good type checking wouldn't have helped. I had removed some code that looked redundant but was actually resolving the aliased name of a type.

### Compile in every direction

With that done I had sat down for some well earned farting around. Last week I had knocked up a simple [shadertoy](https://www.shadertoy.com/) like thing[0] so I went back to working through that.

I got to this part where it was recommending going and implementing various curve functions from folks like [IQ](http://www.iquilezles.org/) & [Golan Levin](http://www.flong.com/). So dutifully I started.. but I was getting this nagging feeling that I should have more noise functions. So after porting some functions from the chaps above I found [this article] (https://briansharpe.wordpress.com/2011/10/01/gpu-texture-free-noise/) on texture-free noise.

Needless to say I got hooked on that, and then finding out out he had a whole [library](https://briansharpe.wordpress.com/2011/10/01/gpu-texture-free-noise/) I decided that I needed it, all of it, in lisp :)

So that was the weekend. I polished up my naive GLSL -> Lisp translator and got cracking porting. Having this much code pouring in was a really good test-case for the compiler and so I got to clean up a few more bugs in the process.

One interesting addition to the compiler was to allow floating point numbers as strings in the lisp code. It looks like this: `(+ (v4! 1.5) (v4! "1.4142135623730950488016887242097"))` the reason for this is that some floating point numbers used in shaders are used for their exact bit-pattern rather than strictly for the value itself. This optional string syntax avoid the risk of the host lisp using a different floating point representation a messing up the value in some way.

The first pass of the import is done. Next is cleanup and documentation but that is less critical. My standard library [Nineveh](https://github.com/cbaggers/nineveh/tree/master/noise) is looking slightly better stocked now :)

### Validation

One thing that felt great was noticing that, in the  naive GLSL -> Lisp translator and got cracking porting. Having this much code pouring in was a really GLSL noise library, there were cases where it would be nice to switch out the hashing function used in a noise function. In the GLSL it was done with comments but I realized this was a *PERFECT* place to use first class functions. So here it is:

```
(defun-g ctoy-quad-frag ((uv :vec2)
                         &uniform (now :float)
                         (mouse :vec2)
                         (mouse-norm :vec2)
                         (mouse-buttons :vec2)
                         (screen-res :vec2))
  ;; here is the hash function we pass in ↓↓↓↓↓↓
  (v3! (+ 0.4 (* (perlin-noise #'sgim-qpp-hash-2-per-corner
                               (* uv 5))
                 0.5))))
```

In that code we call `perlin-noise` passing in the hashing function we would like it to use. And here is the generated GLSL.

```
// vertex-stage
#version 450

uniform float NOW;
uniform vec2 MOUSE;
uniform vec2 MOUSE_NORM;
uniform vec2 MOUSE_BUTTONS;
uniform vec2 SCREEN_RES;

vec3 CTOY_QUAD_FRAG(vec2 UV);
float PERLIN_NOISE(vec2 P);
vec2 PERLIN_QUINTIC(vec2 X2);
vec4 SGIM_QPP_HASH_2_PER_CORNER(vec2 GRID_CELL, out vec4 return_1);
vec4 QPP_RESOLVE(vec4 X1);
vec4 QPP_PERMUTE(vec4 X0);
vec4 QPP_COORD_PREPARE(vec4 X);

vec4 QPP_COORD_PREPARE(vec4 X)
{
    return (X - (floor((X * (1.0f / 289.0f))) * 289.0f));
}

vec4 QPP_PERMUTE(vec4 X0)
{
    return (fract((X0 * (((34.0f / 289.0f) * X0) + vec4((1.0f / 289.0f))))) * 289.0f);
}

vec4 QPP_RESOLVE(vec4 X1)
{
    return fract((X1 * (7.0f / 288.0f)));
}

vec4 SGIM_QPP_HASH_2_PER_CORNER(vec2 GRID_CELL, out vec4 return_1)
{
    vec4 HASH_0;
    vec4 HASH_1;
    vec4 HASH_COORD = QPP_COORD_PREPARE(vec4(GRID_CELL.xy,(GRID_CELL.xy + vec2(1.0f))));
    HASH_0 = QPP_PERMUTE((QPP_PERMUTE(HASH_COORD.xzxz) + HASH_COORD.yyww));
    HASH_1 = QPP_RESOLVE(QPP_PERMUTE(HASH_0));
    HASH_0 = QPP_RESOLVE(HASH_0);
    vec4 g_G5388 = HASH_0;
    return_1 = HASH_1;
    return g_G5388;
}

vec2 PERLIN_QUINTIC(vec2 X2)
{
    return (X2 * (X2 * (X2 * ((X2 * ((X2 * 6.0f) - vec2(15.0f))) + vec2(10.0f)))));
}

float PERLIN_NOISE(vec2 P)
{
    vec2 PI = floor(P);
    vec4 PF_PFMIN1 = (P.xyxy - vec4(PI,(PI + vec2(1.0f))));
    vec4 MVB_0;
    vec4 MVB_1;
    MVB_0 = SGIM_QPP_HASH_2_PER_CORNER(PI,MVB_1); // <<<<<<<<<<<<<<<<<<<
    vec4 GRAD_X = (MVB_0 - vec4(0.49999));
    vec4 GRAD_Y = (MVB_1 - vec4(0.49999));
    vec4 GRAD_RESULTS = (inversesqrt(((GRAD_X * GRAD_X) + (GRAD_Y * GRAD_Y))) * ((GRAD_X * PF_PFMIN1.xzxz) + (GRAD_Y * PF_PFMIN1.yyww)));
    GRAD_RESULTS *= vec4(1.4142135623730950488016887242097);
    vec2 BLEND = PERLIN_QUINTIC(PF_PFMIN1.xy);
    vec4 BLEND2 = vec4(BLEND,(vec2(1.0f) - BLEND));
    return dot(GRAD_RESULTS,(BLEND2.zxzx * BLEND2.wwyy));
}

vec3 CTOY_QUAD_FRAG(vec2 UV)
{
    vec4(MOUSE_NORM.x,MOUSE_BUTTONS.x,MOUSE_NORM.y,float(1));
    return vec3((0.4f + (PERLIN_NOISE((UV * float(5))) * 0.5f)));
}

void main()
{
    CTOY_QUAD_FRAG(<dummy v-vec2>);
    gl_Position = vec4(float(1),float(2),float(3),float(4));
    return;
}
```

I'm stoked at how natural the function call looks in the resulting code. Also it's extra func that the function passed in has 2 return values and everything just worked :)

For completeness here are a couple of the other functions involved

```
(defun-g perlin-noise ((hash-func (function (:vec2) (:vec4 :vec4)))
                       (p :vec2))
  ;; looks much better than revised noise in 2D, and with an efficent hash
  ;; function runs at about the same speed.
  ;;
  ;; requires 2 random numbers per point.
  (let* ((pi (floor p))
         (pf-pfmin1 (- (s~ p :xyxy) (v! pi (+ pi (v2! 1.0))))))
    (multiple-value-bind (hash-x hash-y) (funcall hash-func pi)
      (let* ((grad-x (- hash-x (v4! "0.49999")))
             (grad-y (- hash-y (v4! "0.49999")))
             (grad-results
              (* (inversesqrt (+ (* grad-x grad-x) (* grad-y grad-y)))
                 (+ (* grad-x (s~ pf-pfmin1 :xzxz))
                    (* grad-y (s~ pf-pfmin1 :yyww))))))
        (multf grad-results (v4! "1.4142135623730950488016887242097"))
        (let* ((blend (perlin-quintic (s~ pf-pfmin1 :xy)))
               (blend2 (v! blend (- (v2! 1.0) blend))))
          (dot grad-results (* (s~ blend2 :zxzx) (s~ blend2 :wwyy))))))))
```

```
(defun-g sgim-qpp-hash-2-per-corner ((grid-cell :vec2))
  (let (((hash-0 :vec4)) ((hash-1 :vec4)))
    (let* ((hash-coord
            (qpp-coord-prepare
             (v! (s~ grid-cell :xy) (+ (s~ grid-cell :xy) (v2! 1.0))))))
      (setf hash-0
            (qpp-permute
             (+ (qpp-permute (s~ hash-coord :xzxz)) (s~ hash-coord :yyww))))
      (setf hash-1 (qpp-resolve (qpp-permute hash-0)))
      (setf hash-0 (qpp-resolve hash-0)))
    (values hash-0 hash-1)))
```

All credit of implementation goes to Brian Sharpe for this excellent noise library

### Peace

OK I'll stop now and spare you more details. I'm just so happy to see this behaving.

Have a great week.

Ciao

[0] [https://github.com/cbaggers/fraggle/blob/master/main.lisp](https://github.com/cbaggers/fraggle/blob/master/main.lisp)
