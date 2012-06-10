---
layout: post
title: Finding out you OpenGL Version under Ubuntu
description:
category:
tags: ["opengl", "ubuntu"]
---

If you need to quickly find out the version of OpenGL you have along with the version of GLSL you can use then run the following at the command line.

glxinfo | grep -e "OpenGL.*version string"

Remember that if you have 2 graphics cards that you will need to be aware of which one it will be checking. For me that meant running:

optirun glxinfo | grep -e "OpenGL.*version string"

As I am using [Bumblebee](https://github.com/Bumblebee-Project/Bumblebee/wiki) to enable use of the optimus graphics setup.