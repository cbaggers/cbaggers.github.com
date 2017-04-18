---
layout: post
title: Geometry Shader Shenanigans
description:
date: 2017-04-18 11:59:50
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

When I gave that talk a few weeks back I said, rather naively in hindsight, that geometry & tessellation shaders should work.. man was I wrong there. It turned out to be rather fiddly to find a balance that felt lispy, worked with my current analogies and worked across all GLSL version (or at least failed gracefully).

Lets start at the beginning.

### Passing values between GLSL stages

To pass values from stage to stage in GLSL starts simply, you declare something as `out` in one stage and `in` in the next e.g:

```
out vec4 foo; // in the vertex shader
```
```
in vec4 foo; // in the fragment shader
```

Nice and simple. It also works for arrays of values.

Next we add a geometry (or tessellation) shader. Now we are working with primtives (or patches) so the `out`s from the last stage become an array of `in`s in the geometry stage.

```
out vec4 foo; // in the vertex shader
```
```
in vec4[2] foo; // in the geometry shader
```

The length of the array is dictate by the size of the primitive, so `lines` are length 2, `triangles` are length 3, etc.

But how about if the `out` was an array?

```
out vec4[10] foo; // in the vertex shader
```
```
in vec4[2][10] foo; // in the fragment shader
```

Simple right? I thought so and I updated my compiler to work with this. I kept having this nagging feeling though, something about interface blocks. Turns out I should have listened to the feeling sooner. Support for arrays of arrays only arrive in GLSL in v4.3..shit. Before that you needed to use and interface block:

```
out VertexOuts
{
	vec4[10] foo;
}
```

```
out VertexOuts
{
	vec4[10] foo;
} gs_in[2];
```

Which seems ok, but it has subtleties to it that make this hard to abstract for all GLSL versions. Lets have a look at the lisp.

First a vertex shader:

```
(defun-g test-vert ((position :vec4) (uv :vec2))
  (values position normal))
```

the outputs from the above are a `vec4` which is used as the `gl-position` and a `vec2` which is passed to the next stage. Here is a valid fragment shader to go with this:

```
(defun-g test-frag ((uv :vec2) &uniform (tex :sampler-2d))
  (texture tex uv))
```

So far, so simple. Next let's look at a geometry shader that would match that vertex shader's outputs. We will assume we are rendering triangles.

```
(defun-g test-geom ((uv (:vec2 3)))
  ...)
```

Makes sense right? We just array the inputs. However we know that our interface block is going to cause problems. In this case `uv` isn't really `vec2[3]` it's `in blockName { vec2 }gs_in[3]`. We have a mismatch between the abstraction and reality. However it's a useful lie, it takes the GLSL behavior and makes it easy to understand, so can we keep it.

The answer (at least for now) is to make an 'ephemeral' type, this is a type that can't exists in GLSL for real, but one that our compiler can handle.

So this:

```
(defun-g test-geom ((uv (:vec2 3)))
  (let ((a uv)
        (index 1))
    (aref uv index)
	..))
```

becomes something like:

```
..
in FROM_VERTEX
{
   vec2 uv;
} gs_in[3]

void main()
{
    int index = 1;
    gs_in[index].uv;
    ..
}
```

Notice that `int index = 1` ended up in the GLSL but there is no `a = <something>`, that is because of that ephemeral type. The reason that it has to be ephemeral is that until we use `aref` we can't access the `uv` slot inside the block. Also you cant write `FROM_VERTEX tmp = gs_in[1]` or `FROM_VERTEX[3] tmp = gs_in`, interface blocks look like structs, but they aren't structs and those two example as simply illegal in GLSL, which is a damn shame otherwise this would be much easier!

Instead of this array business we could invent a 'primitive' type. So the geom shader could be:

```
(defun-g test-geom ((uv triangle))
  (let ((a uv)
        (index 1))
    (aref uv index)
	..))
```

But then `test-geom` is no longer a stand alone function, we would need to wait until `test-geom` was used in a pipeline to know what slots `triangle` contained. One of the beauties of CEPL is that all gpu functions are simply functions until they are used in a pipeline, they can be used as stages or just be called from other gpu functions. If we use `triangle` then we can't compile this gpu function ahead of time, which would such as that feature currently lets you find and fix bugs sooner. On top of this `triangle` would also have to be ephemeral as the type doesn't exist in GLSL

The same also goes for requiring the user to define the interface between vertex & geom shaders as structs. You still need the ephemeral & you require the user to repeat themselves.

This is one of those cases where every option is kinda sucky but there is a less sucky option, and that is the first one described and is what I ended up going for.

### Implement it

The above took a few days of experiments and pain to get ironed out, and then it needed to be implemented. As usual working in areas of the compiler that haven't been touched for a while uncovered bugs and general weaknesses that needed fixes.

Another thing that was playing a lot on my mind was that one feature in CEPL that Varjo has support for is defining a stage as raw GLSL and using it in a pipeline, so whatever I made needed to not break that. Having users is awesome and I really want to make sure I don't fuck up their workflow if I can help it. For example one person is already using the GLSL stage support in CEPL to use Tessellation shaders, which is something I've never used! The fact that they can do this makes me happy and eases the pain whilst I slowly find nice lispy ways of doing the same.

Oh, I almost forgot..

### Primitives!

Yes, as we are in the world of Geometry & Tessellation we need to talk about primitives. One of the things I love about Varjo is that it can pass information from stage to stage so the user doesn't have to write the same stuff multiple times, we should be able to do the same with primitives. I wanted to be able to take the OpenGL draw-mode and pass it through the compilation. This allows Varjo to not only write some of the GLSL automatically (e.g. `layout(triangles) in;` in the Geometry stage) but also to know the length of the array'd interface block going into the geometry stage. Having little details like this checked is worth my time as then Varjo can give much nicer and more targeted errors that GL can.

I'll skip the details on this for now as this post is already getting long.

### Where are we now?

We are here

![norms](https://cloud.githubusercontent.com/assets/1579326/25100902/4d6c89f8-23b2-11e7-971b-50aafd92e89e.png)

I finally got these damn things working. Here we are using a geometry shader to draw the normals of the sphere.

There is still a bunch of stuff to do and there are some aspects that really pushed the limits of how 'lispy' stuff could be made. I'll save that for another post. I'll simply say thanks for reading and leave you with the pipeline for drawing the lines in the above.

Ciao

-------------------------

This lisp code:
```
(defun-g normals-vert ((vert g-pnt) &uniform (model->clip :mat4))
  (values (* model->clip (v! (pos vert) 1))
          (s~ (* model->clip (v! (norm vert) 0)) :xyz)))

(defun-g normals-geom ((normals (:vec3 3)))
  (declare (varjo:output-primitive :kind :line-strip :max-vertices 6))
  (labels ((gen-line ((index :int))
             (let ((magnitude 0.2))
               (setf gl-position (gl-position (aref gl-in index)))
               (emit-vertex)
               (setf gl-position
                     (+ (gl-position (aref gl-in index))
                        (* (v! (aref normals index) 0f0)
                           magnitude)))
               (emit-vertex)
               (end-primitive)
               (values))))
    (gen-line 0)
    (gen-line 1)
    (gen-line 2)
    (values)))

(defun-g normals-frag ()
  (v! 1 1 0 1))

(def-g-> draw-normals ()
  :vertex (normals-vert g-pnt)
  :geometry (normals-geom (:vec3 3))
  :fragment (normals-frag))
```

makes this glsl:
```
("#version 450

in vec3 fk_vert_position;
in vec3 fk_vert_normal;
in vec2 fk_vert_texture;

out _FROM_VERTEX_
{
    out vec3 _VERTEX_OUT_1;
};

uniform mat4 MODEL_62CLIP;

void main() {
    vec3 return1;
    vec4 g_G1550 = (MODEL_62CLIP * vec4(fk_vert_position,float(1)));
    return1 = (MODEL_62CLIP * vec4(fk_vert_normal,float(0))).xyz;
    gl_Position = g_G1550;
    vec3 g_G1552 = return1;
    _VERTEX_OUT_1 = g_G1552;
    return;
}
```
```
#version 450

layout (triangles) in;

in _FROM_VERTEX_
{
    in vec3 _VERTEX_OUT_1;
} inputs[3];

layout (line_strip, max_vertices = 6) out;

void GEN_LINE(int INDEX);

void GEN_LINE(int INDEX) {
    float MAGNITUDE = 0.2f;
    gl_Position = gl_in[INDEX].gl_Position;
    EmitVertex();
    gl_Position = (gl_in[INDEX].gl_Position + (vec4(inputs[INDEX]._VERTEX_OUT_1,0.0f) * MAGNITUDE));
    EmitVertex();
    EndPrimitive();
}

void main() {
    GEN_LINE(0);
    GEN_LINE(1);
    GEN_LINE(2);
}
```
```
#version 450

layout(location = 0) out vec4 _FRAGMENT_OUT_0;

void main() {
    vec4 g_G1553 = vec4(float(1),float(1),float(0),float(1));
    _FRAGMENT_OUT_0 = g_G1553;
    return;
}
```
