---
layout: post
title: 2 days late again
description:
date: 2017-06-01 11:12:49
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

But there is news at least!

### Varjo

I changed varjo's `if` form so that if certain conditions are right it will emit a ternary operator instead of a full `if` statement in the glsl

```
(glsl-code
		  (translate
		   (make-stage :vertex '((a :int)) nil '(:450)
					   '((let ((x (if (< a 10)
									   a
									   (- a))))
						   (v! 1 2 3 4))))))
"// vertex-stage
#version 450

layout(location = 0)  in int A;

void main()
{
    int X = (A < 10) ? A : (- A);
    gl_Position = vec4(float(1),float(2),float(3),float(4));
    return;
}
"
```

Aside from this I fixed a few bugs and made little tweaks such as; if you use `->` in a name in lisp it becomes `_to_` in glsl. A tiny touch but it makes the generated code nicer to read.

### Nineveh

This last week I added color space conversion gpu-functions and also functions for generating mesh data for common primitives (sphere, box, cone, etc)

### Streaming

Here are my last 2 streams. I'm getting more used to this each time and it's lovely seeing a few regulars in the chat, this week was especially lovely as there were lots of questions. Not much else to say on this other than I'm gonna try and keep up the 'stream every Wednesday' goal.

<iframe width="560" height="315" src="https://www.youtube.com/embed/i66wYiG_5bk" frameborder="0"></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/de_lwAnD9HE" frameborder="0"></iframe>

Ciao
