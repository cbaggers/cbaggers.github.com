---
layout: post
title: Raining Features
description:
date: 2017-11-27 09:08:01
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Every now and again people ask about if CEPL will support compute and I've always said that it would happen for years. The reason is that, because compute is not part of the GL's draw pipeline, I thought there would be a tonne of compute specific changes that would need to be made. Turns out I was wrong.

Wednesday I was triaging some tickets in the CEPL repo and saw the one for compute. I was about to ignore it when I thought that it would be nice to read through the GL wiki to see just how horrendous a job it would be. 5 minutes later I'm rather disconcerted as it looked easy.. temptingly easy.

'Luckily' however I really want SSBOs for writing out of compute and *that* will be hard .. `looks at gl wiki again` .. Shit.

Ok so SSBOs turned out to be a much smaller feature than expected as well. So I gave myself 24 hours, from late Friday to late Saturday to implement as many new features (sanely) as I could.

Here are the results:

### Sync Objects

GL only has one of these, the fence. CEPL now has support for this too.

You make a fence with `make-gpu-fence`

```
(setf some-fence (make-gpu-fence))
```

and then you can wait on the fence

```
(wait-on-gpu-fence some-fence)
```

optionally with a timeout

```
(wait-on-gpu-fence some-fence 10000)
```

also optionally flushing

```
(wait-on-gpu-fence some-fence 10000 t)
```

Or you can simply check if the fence has signalled

```
(gpu-fence-signalled-p some-fence)
```

### Query Objects

GL has a range of queries you can use, we have exposed them as structs you can create as follows:

```
(make-timestamp-query)
(make-samples-passed-query)
(make-any-samples-passed-query)
(make-any-samples-passed-conservative-query)
(make-primitives-generated-query)
(make-transform-feedback-primitives-written-query)
(make-time-elapsed-query)
```

To begin querying into the object you need to make the query active. This is done with `with-gpu-query-bound`

```
(with-gpu-query-bound (some-query)
  ..)
```

After the scope of `with-gpu-query-bound` the message to stop querying is in the gpu's queue, however the results are not available immediately. To check if the results are ready you can use `gpu-query-result-available-p` or you can use some of the options to `pull-gpu-query-result`, let's look at that function now.

To get the results to lisp we use `pull-gpu-query-result`. When called with just a query object it will block until the results are ready:

```
(pull-gpu-query-result some-query)
```

We can also say not to wait and CEPL will try to pull the results immediately, if they are not ready it will return nil as the second return value

```
(pull-gpu-query-result some-query nil) ;; the nil here means don't wait
```

### Compute

To use compute you simply make a gpu function which takes no non-uniform arguments and always returns `(values)` (a void function in C nomenclature) and then make a gpu pipeline that only uses that function.

```
(defstruct-g bah
  (data (:int 100)))

(defun-g yay-compute (&uniform (woop bah :ssbo))
  (declare (local-size :x 1 :y 1 :z 1))
  (setf (aref (bah-data woop) (int (x gl-work-group-id)))
        (int (x gl-work-group-id)))
  (values))

(defpipeline-g test-compute ()
  :compute yay-compute)
```

You can the `map-g` over this like any other pipeline..

```
(map-g #'test-compute (make-compute-space 10)
      :woop *ssbo*)

```

..with one little difference. Instead of taking a stream of vertices we now take a compute space. This specify the number of 'groups' that will be working on the problem. The value has up to 3 dimensions so `(make-compute-space 10 10 10)` is valid.

We also soften the requirements around gpu-function names for the compute stage. Usually you have to specify the full name of a gpu function due to possible overloading e.g. `(saturate :vec3)` however as compute shaders can only take uniforms, and we don't offer overloading based on uniforms, there can only be one with a given name. Because of this we allow `yay-compute` instead of `(yay-compute)`.

### SSBOs

The eagle eyed of you will have noticed the `:ssbo` qualifier in the `woop` uniform argument. SSBOs give you storage you can write into from a compute shader. Their api is almost identical to that of UBOs so I copied-pasted that code in CEPL and got SSBOs working. This code will most likely be unified again once I have fixed some details with binding however for now we have something that works.

This means we can take our struct definition from before:

```
(defstruct-g bah
  (data (:int 100)))
```

and make a gpu-array

```
(setf *data* (make-gpu-array nil :dimensions 1 :element-type 'bah))
```

and then make an SSBO from that

```
(setf *ssbo* (make-ssbo *data*))
```

And that's it, ready to pass to our compute shader.

### .Phew.

Yeah. So all of that was awesome, I'm really glad to have a feature land that I wasnt expecting to add for a couple more years. Of course there are bugs, the most immediately obvious is that when I tried the example above I was getting odd gaps in the data in my SSBO

```
TEST> (pull-g *data*)
(((0 0 0 0 1 0 0 0 2 0 0 0 3 0 0 0 4 0 0 0 5 0 0 0 6
   0 0 0 7 0 0 0 8 0 0 0 9 0 0 0 0 0 0 0 0 0 0 0 0 0
   0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
   0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0)))
```

The reason for this that I didnt understand layout in GL properly. This also means CEPL doesnt handle it properly and that I have a bug :) (this bug)[https://github.com/cbaggers/cepl/issues/193].

Fixing this is interesting as it means that, unless we force you to make a different type for each layout e.g.

```
(defstruct-g bah140 (:layout :std140)
  (data (:int 100)))

(defstruct-g bah430 (:layout :std140)
  (data (:int 100)))
```

..which feels ugly, we would need to support multiple layouts for each type. Which means the accessor functions in lisp would need to switch on this fact dynamically. That sounds slow to me when trying to process a load of foreign data quickly.

I also have a nagging feeling that the current way we marshal struct elements from c-arrays is not ideal.

This things together make me think I will be making some very breaking changes to CEPL's data marshaling for the start of 2018.

This stuff needs to be done so it's better we rip the band-aid off whilst we have very few known users.

News on that as it progresses.

### Yay

All in all though, a great 24 hours. I'm currently learning how to extract std140 layout info from varjo types so that will likely be what I work on next Saturday.

Peace
