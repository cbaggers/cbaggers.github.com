---
layout: post
title: TaleSpire Dev Log 447
description:
date: 2024-07-25 20:43:57
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks.

Today, I continued with the dice engine.

For those who didn't see yesterday's post, here is a warning:

> GMs, Players, and even implementors of rule systems will not have to deal with the things I’m talking about below. The code gubbins I’ll be showing is for the under-the-hood implementation that will make the nice, user-friendly stuff work.

Currently, the available operations in the language are:

Math:
```
+
-
=
>
>=
<
<=
abs
floor
ceil
round
*
/
expt
mod
remainder
```

Functions and [special-forms](https://courses.cs.northwestern.edu/325/readings/special-forms.html):
```
dice - Creates a dice set
roll - Puts the dice passed to this function into the dice tray for a user to roll
sort - sorts dice based on their score given a user-provided predicate
take-n - make a new dice-set containing the first N dice from another dice-set
take-if - make a new dice-set containing any dice from the given dice-set that satisfy a predicate
drop-if - make a new dice-set without any dice from the given dice-set that satisfy a predicate
high-face - return the 'highest' face for a given die (e.g. 6 for a d6)
low-face - return the 'lowest' face for a given die (e.g. 1 for a d6)
count - returns the number of dice in a dice-set
show - Push an entry into the dice history
let - for lexically defining variables and functions
if - it's a conditional
recurse - for calling the function you are in, only works from the tail position
```

This allowed me to implement:

- various kinds of exploding dice
- fate dice
- re-rolling on various criteria
- more complete math functionality
- counting the number of dice that satisfied some criteria

Many of the functions support dice-sets as well as numbers, so 2d6+5 would compile to `(+ (roll (dice d6 2)) 5)`.

It's going rather well. Next on the todo-list, I'd like to implement currying, clean things up, and start implementing the compiler.

I'm also considering using fixed-point math instead of floats, and I'm looking into combining recursion with re-rolls[0].

This will have to wait as I really need to do some work for TitanCraft tomorrow. They've been waiting patiently for me to sort out some bugs, and this week has gotten away from me.

We'll get back to dice by the middle of next week for sure.



*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] This sounds weird. However, we don't want infinite loops, and this would mean the code would always yield to the user so they can cancel the roll if it goes on for too long.
