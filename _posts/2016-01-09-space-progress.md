---
layout: post
title: Space progress
description:
date: 2016-01-09 11:48:04
category:
tags: ['lisp', 'cepl']
---

Having got something working on the shader generation side I have gone back to the cpu side of the story.

When the shader compiler sees a position changing space like this:

	(in s
	  (in w
		(p! (v! 1 1 1 1)))
	  0)

it turns it into glsl something like this:

    W-TO-S * vec4(1,1,1,1)

Where W-TO-S is a matrix4 which is to be uploaded. On the cpu side cepl adds something like this

	(let ((w-to-s (spaces:get-transform w s)))
      (cepl-gl::uniform-matrix-4ft w-to-s-uniform-location w-to-s))

Up until now the `get-transform` function has just returned the identity matrix, so it was time to flesh that out.

I now have a implementation that looks like it works but is incredibly inefficient. It will do the job for now, and as cepl is already uploading the result, the feature should be working now.

This leaves the question of what good code looks like when using this feature. I have a few things in my mind already that need answering:

- What happens if you try to pass a ephemeral type like position or space from one shader stage to another?
- Should stages have an implicit space (I think so)
- Can ephemeral types be used in gpu-structs (I think this should be made to work)
- If you pass up a position as a uniform, where does it's space come from?
- If you made a gpu-struct slot a position, should the space come from the stage?
- If there are implicit spaces in shaders then we need implicit uniform upload. Do we just use the implicit-uniforms feature for this? How does that interact with the compiler's space-transform pass.

This should keep me busy :)
