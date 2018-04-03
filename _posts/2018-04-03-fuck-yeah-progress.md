---
layout: post
title: Fuck yeah progress!
description:
date: 2018-04-03 22:28:14
category:
tags: ['lisp', 'cepl', 'varjo']
---

I've been on holiday for a week and it's been great for productivity; the march to get stuff out my head before my new job goes rather well.

## More docs

First stop [rtg-math](https://github.com/cbaggers/rtg-math). This is my math library which for the longest time has not been documented I fixed that this week so now we have a [bootload of reference docs](http://techsnuffle.com/rtg-math/rtg-math-reference.html). Once again [staple](https://shinmera.github.io/staple/) is hackable enough that I could get it to produce signatures with all the type info too. Blech work but I can not think about that for a while. One note is that the 'region' portion of the api is undocumented as that part is still WIP.

## SDFs

Next I wanted to get some signed distance functions into [Nineveh](https://github.com/cbaggers/nineveh) (which is my library of useful gpu functions). SDFs are just a cool way to draw simple things and so with the help of code from shadertoy (props to iq, thoizer & Maarten to name a few) I made a nice little api for working with these functions. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/MPKPAzRx1ZM" frameborder="0"></iframe>

One lovely thing was in the function that handles the shadows, it needs to sample the distance function at various points and rather than hardcoding it we are able to pass it in as a function. As a result we get things like this:

    (defun-g point-light ((fn (function (:vec2) :float))
                          (p :vec2)
                          (light-position :vec2)
                          (light-color :vec4)
                          (light-range :float)
                          (source-radius :float))
      (shaped-light fn
                    #'(length :vec2)
                    p
                    light-position
                    light-color
                    light-range
                    source-radius))
                    
Here we have the point-light function, it takes a function from `vec2` to `float` and some properties and then calls `shaped-light` passing in not just this but also a function that describes the distance from the light source (allowing for some funky shaped lights, though this is WIP). 

This made me super happy as using first class functions in shaders as a means of composition had been a goal and seeing more validation of this was really fun.

## Particle Graphs

One thing with these functions is when you want to visualize them it's a tiny bit tricky as for each point you have a distance, which makes 3 dimensions. We can plot the distance as a color but it requires remapping negative values and the human eye isnt as good as differentiating color as position.

To help with this I made a little particle graph system.

<iframe width="560" height="315" src="https://www.youtube.com/embed/mXjyJgPblrY" frameborder="0"></iframe>

`define-pgraph` is simply a macro that generates a pipelines that uses instancing to plot particles, however it was enough to make visualizing some stuff super simple. However as they are currently just additive particles it is difficult to judge which are in front sometimes. I will probably parameterize that in future.

## Live recompiling lambda-pipelines

In making this I realized that there was a way to make CEPL's lambda pipelines[0] be able to respond to recompilation of the functions they depend on without any visible api change for the users. This was clearly to tempting to pass up so I got that working. 

The approach is as follows (a bit simplified but this is the gist):

    (let ((state (make-state-struct :pipeline pline    ;; [1]
                                    :p-args p-args)))  ;; [2]
      (flet ((recompile-func ()
               (setf (state-pipeline state)            ;; [4]
                     (lambda (&rest args)
                       (let ((new-pipeline
                              (recompile-pipeline      ;; [5]
                               (state-p-args state)))) ;; [6]
                         (setf (state-pipeline state)  ;; [7]
                               new-pipeline)
                         (apply new-pipeline args))))));; [8]
        (setf (state-recompiler state) #'recompile-func) 
        (values
         (lambda (context stream &rest args)           ;; [3]
           (apply (state-pipeline state) context stream args))
         state)))

`[1]` first we are going the take or pipeline function (`pline`) and box it inside a struct along with the arguments `[2]` (`p-args`) used to make the pipeline. If we hop down to the final lambda we see that this `state` object is lexically captured and the boxed function unboxed and called each time the lambda is called. Cool so we have boxed a lambda, however we need to be able to replace that inner pipeline whenever we want.

To do this we have the `recompile-func`, ostensibly we call this to recompile the inner pipeline function, however there is a catch: the recompile function could be called from any thread. Threads are not the friend of GL so we instead `recompile-func` actually replaces the boxed pipeline `[4]` with the actual lambda that will perform the recompile. As it is only valid for the pipeline to be called from the correct thread we can safely assume this. So next time the pipeline is called it's actually the recompile lambda that will be called; this finally does the recompile `[5]` (using the original args we cached at the start `[6]`) and then replaces itself in the state object with the new pipeline function `[7]`. As the user was also expecting rendering to happen we call the new pipeline as well `[8]`.

There are trade-offs of course, this indirection will cost some cycles and as we don't know how the signature of the inner pipeline function will change we can't type it as strictly as we would want to otherwise. *However* we can opt out of this behavior by passing `:static` to the `pipeline-g` function when we make a lambda pipeline, this way we get the speed (and no recompilation) when we need it (e.g. when we ship a game) and consistency and livecoding when we want it (e.g. during development).

Needless to say I'm pretty happy with this trade.

## vari-describe

`vari-describe` is a handy function which returns the official glsl docs if available. This was handy but I have a growing library of gpu functions that naturally have no glsl docs.. what to do? The answer of course is to make sure docstring defined inside these are stored by varjo per overload so they can be presented. In the event of no docs being present we can at least return the signatures of the gpu function which, as long as the parameter names were good, can be quite helpful in itself.

This took a few rounds of bodging but works now. A nice thing also is that if you add the following to your `.emacs` file:

    (defun slime-vari-describe-symbol (symbol-name)
      "Describe the symbol at point."
      (interactive (list (slime-read-symbol-name "Describe symbol: ")))
      (when (not symbol-name)
        (error "No symbol given"))
      (let ((pkg (slime-current-package)))
        (slime-eval-describe
         `(vari.cl::vari-describe ,symbol-name nil ,pkg))))

    (define-key lisp-mode-map (kbd "C-c C-v C-v") 'slime-vari-describe-symbol)
    
You can hold control and hit `C V V` and you get a buffer with the docs for the function your cursor was over.

This all together made the coding experience significantly nicer so I'm pretty stoked about that.

## Tiling Viewport Manager

This is a project I tried a couple of years back and shelved as I got confused. In emacs I have a tonne of files/programs open in their respective buffers and the emacs window can be split into frames[9] in which the buffers are docked; this way of working is great and I wanted the same in GL.

The idea is to have a standalone library that handles layouting of frames in the window into which 'targets' (our version of buffers) are docked. These targets can either just be a CEPL [viewport](http://techsnuffle.com/cepl/api.html?#CEPL.VIEWPORTS%3AVIEWPORT) or could be a [sampler](http://techsnuffle.com/cepl/api.html?#CEPL.SAMPLERS%3ASAMPLER) or [fbo](http://techsnuffle.com/cepl/api.html?#CEPL.FBOS%3AFBO). 

As the target is itself just a class you can subclass it and provide your own implementation. One thing I was testing out was making a simple color-picker using this. Here's a wip, but the system cursor is missing from this, the point being 'picked' is at the center of the arc.

![yay](/assets/images/picker.png)

## Back to work

Back to work now so gonna lisping will slow down a bit. In general I'm just happy with this trend, it's going in the right direction and it feels like this could get even more enjoyable as I build up [Nineveh](https://github.com/cbaggers/nineveh) and keep hammering out bugs as people find them.

Peace all, thanks for wading through this!



[0] pipelines in CEPL are usually defined at the top level using `defpipeline-g`, lambda pipelines give us `pipeline-g` which uses Common Lisp's `compile` function to generate a custom lambda that dispatches the GL draw.
[9] in fact emacs has these terms the other way around but the way I stated is usually eaiser for those more familiar with other tools
