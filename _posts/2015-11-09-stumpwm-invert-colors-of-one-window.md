---
layout: post
title: Stumpwm invert colors of one window
description:
date: 2015-11-09 20:47:25
category:
tags: ['lisp', 'cepl']
---

One of the last things I was missing on my new stumpwm setup was being able to invert the colors of a single window. Turns out this is not hard to do.

1. Make a script called `inv.sh` containing the following

#! /bin/bash

	if [ "$(pidof compton)" ];
		then
				pkill compton
		else
				xdotool getactivewindow | xargs -I {} compton --backend glx --invert-color-include 'client={}'
	fi
	exit


2. `chmod +x inv.sh`

3. Add the following to your .stumpwmrc

    (define-key *root-map* (kbd "C-i") "exec /home/baggers/Code/shell/inv.sh")

Boom, done. When you click `<your-prefix> C-i` the currently focused windom will be inverted. Running it again disables the effect.

This is pretty hacky and I'd like to make something more integrated with stumpwm so Ι can have multiple inverted windows at a time. That will be easy enough when it comes to it, but for now this does the job.
