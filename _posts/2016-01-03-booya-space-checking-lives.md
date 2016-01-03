---
layout: post
title: Booya! Space checking lives!
description:
date: 2016-01-03 17:47:20
category:
tags: ['lisp', 'cepl']
---

Oh progress feels so good!

Ok, now given some code like this:

```
(defun-g vert ((vert pos-col) &uniform (s space-g) (w space-g))
  (in s
    (in w
      (p! (v! 1 1 1 1)))
    0)
  (values (v! (pos vert) 1.0)
          (col vert)))
```

The `in` portion does nothing useful, it purly exists so that there is a `position` (the `p!`) which is created inside the scope of the space `w`. That `position` is returned into the scope of space `s`. To ensure this is valid the compile needs to multiple the position by some kind of `w space to s space` matrix.

Lets look at the compile output (it's been cleaned up for readability)

```
#version 330

layout(location = 0) in vec3 fk_vert_position;
layout(location = 1) in vec4 fk_vert_color;

out vec4 OUT_34V;
uniform mat4 W-TO-S;

void main() {
  (W-TO-S * vec4(1,1,1,1));
  0;
  gl_Position = vec4(fk_vert_position,1.0f);
  OUT_34V = fk_vert_color;
}
```

Here we can see that there is no mention of `w`, `s`, `position`s or `spaces`. The compiler has created a uniform to hold the `w space to s space` transform and multiplied it with the vec4.

On the cpu side we can find this code

```
...
(let ((w-to-s (spaces:get-transform w s)))
  (when w-to-s
    (let ((cepl-gl::val w-to-s))
      (when (>= w-to-s-uniform-location 0)
	(cepl-gl::uniform-matrix-4ft w-to-s-uniform-location cepl-gl::val)))))
...
```
Here we see that the original spaces `w` & `s` are passed to the `get-transform` function which returns the matrix4 which is uploaded.

As you would imagine there are still plenty of little bugs to clean up, but those feel like nothing compared to how much work this has taken. I'm pretty stoked right now.

I'm looking forward to posting a proper example of how you would use this in regular shaders soon.

That's enough for this week

Ciao!
