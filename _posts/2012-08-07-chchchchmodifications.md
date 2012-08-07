---
layout: post
title: Ch-ch-ch-ch-modifications
description:
category:
tags: [lisp, cepl, opengl]
---

Well I've been very quiet recently and that's mainly down the my mild addiction to playing with Lisp. I been beavering through  [Let Over Lambda]() which ia fantastic book on the crazy stuff in common lisp (code that writes code etc) which has been bending my mind plenty. 

I've also got sick of how the opengl interface works and have gone on something of a spring clean. I've made some tools on top of the exisiting common lisp opengl library in order to make it a nicer to work with. 

Its definitely not 'lispy' enough yet but pushing data into buffers, making vaos and managing programs and their uniforms is much easier now. I've got it all [up on github]() though I wouldn't recommend playing with it yet as I'm damn sure there will be some HEAVY changes before I'm happy with it.

Here is a quick video of me playing around with a small chunk of code which handles the rotation of the objects in the scene. Notice how errors don't crash the program and I can simply recompile the offending function and carry on. Also note that the repl still works so I can mess around with the camera position from there.

<iframe width="420" height="315" src="http://www.youtube.com/embed/qcahUrvytqs" frameborder="0"></iframe>

I'm now bored of these little 3D whatchamagigs now so I need to write an importer .obj files so I can get something interesting up on the screen.

Ciao