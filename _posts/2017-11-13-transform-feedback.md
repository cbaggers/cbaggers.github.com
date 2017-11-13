---
layout: post
title: Transform Feedback
description:
date: 2017-11-13 09:10:35
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Ah it feels good to have some meat for this week's writeup. In short transform feedback has landed in CEPL and will ship in the next[0] quicklisp release.

### What is it?

Transform feedback is a feature that allows you to write data out from one of the vertex stages into a VBO as well as passing it on to the next stage. It also opens up the possibility of not having a fragment shader at all and just using your vertex shader like the function is a gpu based `map` function. For a good example of it's use check out [this great tutorial by the little grasshopper](http://prideout.net/blog/?tag=opengl-transform-feedback).

### How is it exposed in CEPL?

In your gpu-function you simply add an additional qualifier to one or more of your outputs, like this:

	(defun-g mtri-vert ((position :vec4) &uniform (pos :vec2))
	  (values (:feedback (+ position (v! pos 0 0)))
			  (v! 0.1 0 1)))

Here we see that the gl_position from this stage will be captured, now for the cpu side. First we make a gpu-array to write the data into.

	(setf *feedback-vec4*
		  (make-gpu-array nil :element-type :vec4 :dimensions 100))

And then we make a transform feedback stream and attach our array. (transform feedback streams can have multiple arrays attached as we will see soon)

	(setf *tfs*
		  (make-transform-feedback-stream *feedback-vec4*))

And finally we can use it. Assuming the gpu function above was used as the vertex stage in a pipeline called `some-pipeline` then the code will look like this:

	(with-transform-feedback (*tfs*)
	  (map-g #'prog-1 *vertex-stream* :pos (v! -0.1 0)))

And that's it! now the first result from `mtri-vert` will be written into the gpu-array in `*feedback-vec4*` and you can pull back the values like this:

	`(pull-g *feedback-vec4*)`

If you add the feedback modifier to multiple outputs then they will all be interleaved into the gpu-array. However you might want to write them into seperate arrays, this can be done by providing a 'group number' to the `:feedback` qualifier.

	(defun-g mtri-vert ((position :vec4) &uniform (pos :vec2))
	  (values ((:feedback 1) (+ position (v! pos 0 0)))
			  ((:feedback 0) (v! 0.1 0 1))))

making another gpu-array

	(setf *feedback-vec3*
		  (make-gpu-array nil :element-type :vec3 :dimensions 10))

and binding both arrays to a transform feedback stream

	(setf *tfs*
		  (make-transform-feedback-stream *feedback-vec3*
										  *feedback-vec4*))

You can also use the same pipeline multiple times within the scope of `with-transform-feedback`

	(with-transform-feedback (*tfs*)
	  (map-g #'prog-1 *vertex-stream* :pos (v! -0.1 0))
	  (map-g #'prog-1 *vertex-stream* :pos (v! 0.3 0.28)))

CEPL is pretty good at catching and explaining cases where GL will throw an error such as: not enough vbos (gpu-arrays) bound for the number of feedback targets -or- 2 different pipelines called within the scope of `with-transform-feedback`

### More stuff

During this I ran into some aggravating issues relating to transform feedback and recompilation of pipelines, it was annoying to the point that I rewrote a lot of the code behind the `defpipeline-g` macro. The short version of this is that the code emitted is no longer a top-level closure and also that CEPL now has ways of avoiding recompilation when it can be proved that the gpu-functions in use havent changed.

I also found out that in some cases `defvar` with type declarations is faster than the captured values from a top level closure, even when they are typed. See [here](https://github.com/cbaggers/cepl/blob/master/core/protocode/var-speed-test.lisp) for a test you can run on your machine to see if you get the same kind of results.[1]

### Shipping it

Like I said this code is in the branch to be picked up by the next quicklisp release. This feature will certainly have it's corner cases and bugs but I'm happy to see this out and to have one less missing feature from CEPL.

Future work on transform feedback includes using the [transform feedback objects](https://www.khronos.org/opengl/wiki/Transform_Feedback#Feedback_objects) introduced in GLv4 to allow for more advanced interleaving options and also nesting of the `with-transform-feedback` forms.

Next on the list for this month is shared contexts. More on that next week!

Ciao

[0] late november or early december
[1] testing on my mac has given different results, use `defvar` and packing data in a struct is faster than multiple `defvar`s but slower than the top level closure. Luckily it's still on the order of microseconds a frame (assuming 5000 calls per frame) but measurable. It's interesting to see as packed in `defvar` was faster on my linux desktop `¯\_(ツ)_/¯` I'll show more data when I have it.
