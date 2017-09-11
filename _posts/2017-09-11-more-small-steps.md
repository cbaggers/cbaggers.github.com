---
layout: post
title: More small steps
description:
date: 2017-09-11 09:28:06
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Over the weekend I got a little lisping done and was working on something that has been rolling around my head for a couple of years.

During the standardization process of lisp as well as agreeing on what would go in, there were also things cut. Some of those things have become de facto standards as all the implementations ship them, however some seem rather fundamental.

One of the more fundamental ones that didn't make it was the idea of introspectable (and extensible) [environment objects](http://clhs.lisp.se/Body/03_aad.htm).

The high level view goes something like this: An environment object is a set of lexical bindings, having access to this (and any metadata about those bindings) would allow you to do more semantic analysis of the code. Given that any macro is allowed access to the environment object when evaluated this would allow a macro to expand differently depending on the data in the environment.

For example let's say that we use the environment to store static type information in the environment; we could then potentially optimize certain function calls within that scope using this information (like using static dispatch on a generic function call).

Now a while back stassats kindly shared [a snippet](http://paste.lisp.org/display/310079) which allows you to *essentially* define a variable who's value is available during macro expansion. Over the weekend I've been playing with this to provide a way to allow the following.

```
(defmacro your-macro-here (&environment env x y)
  (with-ext-env (env)
    (augment-environment env :foo 10)
    `(+ x y)))
```

So you can wrap the code in your macro in `with-ext-env` and this lets you get access to a user-extensible environment object. We would then provide functions (like `augment-environment`) to modify the environment, in the above code to store the value `10` against the key `:foo` however we could use this for type info.

The downsides are that we don't get all the data that was potentially available in the [proposed feature](https://www.cs.cmu.edu/Groups/AI/html/cltl/clm/node102.html). I'd really like to have access to all the standard declarations made as well as our additional ones.

Luckily it's possible to make a new CL package with modified versions of `let`, `labels`, etc and in those to capture the metadata and make it available to our extensible environment.

With this we may be able to make something that convincingly does the job of a extensible macro environment. I have made a prototype of the meat (the passing of the environment itself) and so next is it wrap the other things into a package and then see if it is useful.


Other than this I've been poking around a little with Unity. It's fun to see how it's done in the big leagues and to see where ones approach aligns and diverges from a larger player's philosophy.

That's all for now,

Seeya!
