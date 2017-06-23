---
layout: post
title: input given
description:
date: 2017-06-23 09:36:19
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

I'm late writing this week but there has been progress.

So after the PBR fail I knew I had to put some energy in different places for a week as I needed content for the stream. I knew dealing with input was coming soon and my input system was ugly, so that was an obvious candidate.

This is probably the 4th attempt I've made of making an event system. The originals were to varying degrees attempts on the callback/reactive input systems but they suffered a few key problems:

- Callbacks can be hard to reason about when there are enough of them
- Subscribing to something means that the provider now holds a reference to the subscribee, which means it cant be freed if you forget to detach it (this is an issue as we experiment in the repl a lot so throwing things away should be easy)
- Input propagation can end up driving the per frame execution (more on this in a minute)
- In the versions where I made immutable events the allocation costs per frame could be very high[0]

The third one was particularly tricky, and it only became apparent to me when I had some specific use cases:

The first one was with mouse position events. The question is, which event was the last one for that frame. Let's say I'm positioning something based on mouse position and that causes a bunch of work in other parts of the game. If I receive 10 move events in a frame I don't want to do that work 10 times so there is going to be some kind of caching and I need to know when I have received the last event. This is a bit artificial and there are plenty of strategies around it but caching is something that ends up appearing a lot in the event propagating approach.

Next was when I had made a fairly strict entity component system and was trying it out in a little game. In this system you processed the components in batches, so you would process all of the `location` components and then all of the `renderable` components, etc. This posed a problem as events needed to be delivered to components but events were pumped at the beginning of the frame and components were processed later. I didn't want to break the model so again I fell back to caching.

I needed something different and so the latest version, [skitter](https://github.com/cbaggers/skitter), is much simpler.

First some design goals:

- The api is pull based[1]
- There must only be allocations happening when new input-sources (like mice, gamepads) are added or when new controls (like buttons, scroll wheels) are added to input sources.
- skitter shouldn't need to know about any specific input api, it's only dealing in its own state.
- It should be possible to both redefine controls live OR tell the system to make them static and optimize the crap out of them.

In the system you now define a control like this:

```
;;          name ↓↓      lisp data type ↓↓     ↓↓ initial value
(define-control wheel (:static t) single-float 0.0)
```
This is like defining a type in the input system, you give it a name, tell it the kind of data it holds and it's initial value.

the `:static t` bit means this will be statically typed and will not be able to be changed live

this one is slightly different:

```
(define-control relative2 (:static t) vec2 (v! 0 0) :decays t)
```

Here we have told it to 'decay'. This means that each frame the value returns to the initial value. This is because you don't get an event from (all/any?) systems to tell you that something has stopped happening.

We can now make an input source

```
(define-input-source mouse ()
  (pos position2)
  (move relative2)
  (wheel wheel2)
  (button boolean-state *))
```

Here a mouse has an absolute position, a relative movement, a possibly 2d scroll wheel and buttons (we dont know how many so we say `*`)

We can then call `(mouse-pos mouse-obj)` to get the position.

You can then write code that takes the events from your systems (`sdl2`, `qt`, `glfw`, etc) and puts them into the source by using functions like `(skitter:set-mouse-pos mouse-obj timestamp new-position)`

Bam cross system event management. It's dirt simple to use and the hairy logic is all handled by a few macros internally.

BUt we have missed one thing that callbacks were good for, events over time. Reactive approaches have this awesome way of composing streams of events into new events and, whilst I can't have that, I do want something. The caching in our system poses a problem as we now *only* have the latest event. So rather that bringing the events to the processing code we will bring the processing to the event (or something :D).

What we do is add a `logicial-control` to our input-source. Maybe we want double-click for our mouse, we will make a kind of control with it's own internal state that sets itself to true when it has seen two events within a certain timeframe:

```
(define-logical-control (double-click :type boolean :decays t)
    ((last-press 0 integer))
  ((button boolean-state)
   (if (< (- timestamp last-press) 10)
	   (progn
		 (setf last-press 0)
		 (fire t))
	   (setf last-press timestamp))))
```

This is made up & untested code, however how it should work is that we have new kind of control called double-click, it's state is boolean. It has a private field called last-press which holds the time since the last press. It depends on the boolean-state of another control and internally we give this thing the name 'button'. The `if` is going to get evaluated every time one of the dependent controls (`button` in this case) is updated. `fire` is the function you call to in a logical-control to update your own state.

What is nice with this is that you can then define attack combos as little state-machines, put them in logical controls and attach them to the controller object. You can now see the state of Ryu's Shoryuken move in the same way you would check if a button is being held down.

The logical-control is still very much a WIP but I'm happy with the direction it is going.

That's all for input for now, I have been doing other stuff this week like making a tablet app which lets you use sliders and pads to send data to lisp [2] and of course more streaming[3] but it's time for me to go do other stuff.

This weekend I am going to have a go at making a little RPG engine with all this stuff. Should be fun!

[0] you could of course cache event objects, but then you need *n* of each type of event object (because we may dispatch on type) and we can't make them truly immutable.

[1] I made a concession that you can have callbacks as they are used internally and it hopefully means people won't try and hack their own in on top of the system.

[2]
![tablet app](https://everyweeks.com/xI8fR7QIcw4ACYyRWfnwcF1anUF436rN-tapp.jpg)

[3]

<iframe width="560" height="315" src="https://www.youtube.com/embed/CdDVjRVOifM" frameborder="0" />
