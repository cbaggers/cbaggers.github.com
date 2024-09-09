---
layout: post
title: TaleSpire Dev Log 446
description:
date: 2024-07-24 22:53:42
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks!

I've started work on my next big feature, the dice engine!

This is a complete rewrite not just of the networking code but also (and more excitingly) the system that handles the automation of different kinds of dice rolls.

Until now, we've supported very basic dice syntax. For example, you can type `!farble:2d20-2/garble:3d6+2d4` into the chat bar and get a combined roll with 2d20, 3d6, and 2d4. Once you've rolled, it will do the math and show the results with the titles "fable" and "garble" above the respective results.

This is very limited, though; we don't yet support exploding dice, re-rolls, fate dice, or many other useful constructions.

My task is to fix that limitation.

I'm gonna talk about work-in-progress techno hoo-hah for a bit, so a warning:

> GMs, Players, and even implementors of rule systems will not have to deal with the things I'm talking about below. The code gubbins I'll be showing is for the under-the-hood implementation that will make the nice, user-friendly stuff work.

Alrighty! With that out of the way, let's get nerdy.

Many VTTs have a nice, terse dice syntax language that supports a range of useful kinds of rolls.

I want us to have something similar, of course, but eventually, I'd like to be able to support dice rolls written in other systems' syntax, too. This is nice for people who have experience with other VTTs and don't want to learn a new system each time they move.

I see most dice languages like regular expressions, a (nice?) compact way to express a specific class of problem.

However, there is usually a trade-off for that terseness, and it's a loss of generality. For example, if a new game comes out with a new style of roll, you'd have to wait for new syntax to be added. However, with a richer language, the community could implement the new rule and start using it immediately.

Alright, so I want dice languages to be available to the users, but behind the scenes, I want something more flexible.

I also don't want to have to make a whole new backend for each dice language we support. So it's time for compilers \o/

The first step is transforming these strings into an [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree). The AST is a data structure that describes a dice roll. It's much more verbose than but much more general. It supports variables, function definition, recursion, etc.

Here is a contrived example where we roll 6d6, re-roll the two lowest results, and add up the final score[0]

```
(let ((rolled (roll (dice d6 6))))
  (+ (take-top 4 rolled)
     (roll (take-bottom 2 rolled))))
```

Phew, you wouldn't want to have to write that every time! And, of course, you wouldn't, as this representation would be used by TaleSpire behind the scenes. 

Next up, to run it. We could write an [eval](https://en.wikipedia.org/wiki/Eval) function that simply walks the AST and executes it. That's what I've done in my experiments so far, as it was a quick way to get something working.

For release, I think I'd like to take the AST and compile it into byte-code instead. We will then have a small byte-code evaluator to run that code. There are some advantages to this:

- The evaluator is simpler: For example, with tail-call elimination, we should be able to flatten the recursion to jumps.
- Easier to port to other languages: I'd like to port the evaluator to different languages. Having the code be a flat array makes working with it trivial, whether from C++ or Python.
- Trivial to encode for sending as a URL and embedding in rules documents: We have talespire:// URLs and a rule system coming in the future. I like the idea of making it very to interface with those things.

I'm currently experimenting with this system in common-lisp, as that's just a nice place to noodle with ideas. I have an AST and working eval, so I'm able to make dice rolls in the REPL and check the behavior.

The language needs plenty of work, but to guide that, I will implement a bunch of common rolls from various rule systems. When I'm happy that this covers what we need, I'll start on cleanup and then on the byte-code compilation.

This all may sound a bit complicated, but it's actually straightforward to make. The code is way less involved than most things in making games. I recommend playing with toy compilers if you are curious. Once you get past parsing the text, it's a lot of fun.

Okay, that's enough nattering for now. I'll see you in the next one.

p.s. I'd like to open-source the language and compiler once it's been in production for a while. I couldn't find a good place to stick this in the text above, so it's chilling out down here.

[0] Again, this is very much not it's final form. We will be using [s-expressions](https://en.wikipedia.org/wiki/S-expression) for the textual representation of the AST, but take-top and take-bottom feel like they could easily be reworked as a `take` function that itself would take a predicate function to tell it what to take.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
