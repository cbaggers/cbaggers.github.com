---
layout: post
title: Lisp, OpenGL and Uninterrupted development
description:
category:
tags: ['lisp', 'OpenGL', 'study', 'slime']
---

As with anything I want to learn, it seems I need a project to drive me. The project is often WAY above my ability but it makes for a very motivating goal to keep in mind while trudging through the more tedious/mind-braking parts of whatever I'm studying.

The project I have in mind with Lisp is a live game coding environment. I listened to an awesome talk on [FLOSS Weekly](http://twit.tv/show/floss-weekly) a little while back on [Overtone](https://github.com/overtone/overtone), a live music coding environment, written in [Clojure](http://clojure.org). If you are struggling to see how this would work have a look at [this video](http://vimeo.com/22798433).

The real feeling I get here is that, just like in music, coding can have composing and jamming. What I mean is that programming (to me at least) can often feel like I'm composing, I am primarily solitary, noodling around with little ideas and bits of code until I get a good idea of where I'm going and then I need to plan and write so I can eventually present what I have made. 

Jamming has the result visible from the start, and because what is happening is immediately viewable (audible) other can chip in and shape the result, even if they are not playing themselves.

I want to do this with code, specifically visual programming and games. 

These ideas obviously aren't new, they have been said plenty of times before, and there have been strides in the direction. Every time the speed of iterating in a language is improved, we are one step closer to this goal.
Recently a fantastic guy called [Bret Victor](http://worrydream.com/) gave [this amazing talk](http://www.youtube.com/watch?v=PUv66718DII) where he explained that "creators need an immediate connection with what they're creating" and demonstrated it beautifully with (amongst other wicked demos) a 2d platformer where the changes he made in the code were immediately reflected in the game. This seems cool enough but the result of this immediate feedback is that he could 'jam' with the code, he could make changes with worrying about crashing out of the demo and just explore the 'what ifs' of what he was writing.

Now while Lisp doesnt implement this at all it does have one feature that is very nice, you can compile the code for a single function and add it to the running program. This also means you can redefine the code you are running while it's running which is a great first step.

Just a minute ago I was able to load up the OpenGL tutorial I was working on which involved a triangle moving in a circle (yup I'm starting from scratch!) and, just by virtue of the code being written in lisp, I was able to change the colour, path, speed of the triangle and immediately push the changes in the code into the running program and see the effects they had. 

I could also see how things broke if I messed with commands in the wrong way, and thanks to how lisp handles exceptions I could undo the change in the code, push the change into the program and tell the program to continue using the new code, and in only couple of seconds. I must admit, it felt fantastic!

I'm putting at the work I'm doing on github although there' not really much there yet, I will post here with videos and pics as more takes shape. 

Take care, happy jamming!