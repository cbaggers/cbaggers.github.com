---
layout: post
title: Much reading but less code
description:
date: 2016-08-08 12:44:09
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

> TLDR: If you add powerful features to your language you must be sure to balance them with equally powerful introspection

I haven't got much done this week, I have some folks coming from the uk soon so cleaning up and getting ready for that. I have decided to take on more projects though :D

Both revolve around shipping code which was the theme last week as well. At first I was looking at some basic system calls and it seemed that, if there wasn't a good library for this already then I should make one. Turns out that was mostly me not knowing what I'm doing. I hadn't appreciated that the posix api specifies itself based on certain types and that those types can be different size/layouts/etc on different platforms, which means that we can just define lisp equivalents without knowing those details. To get around this we need to use cffi's groveler but this requires you to have a C compiler set up and ready to go. This, in my opinion, sucks as the user now has to think about more than the language they are trying to work in. Also because all libraries are delivered though the package manager you tend not to know about the C dependencies until you get an error during build which makes for pretty poor user experience.

To mitigate this what we can do is cache the output of the C program along with info on what platforms it is valid for. That second part is a little more fiddly as the specification that is given to the `groveler` is slightly different for different platforms and those difference are generally expressed with read-time conditionals. If people used reader conditionals in the specification then the cache is only valid if the result of the reader-conditionals matches. The problem is that the code that doesn't match the condition is never even read so there is nothing to introspect.

One solution would be tell people not to use reader-conditionals and use some other way of specifying the features required, we would make this a standard and we would have to educate people on how to use it.

But `#+` `#-` are just reader macros so we could replace them with our own implementation which would work exactly the same except that it would also record the conditions and the results of the conditions.

This turned out to be REALLY easy!

The mapping between a character pattern like `#+` and the function it calls is kept in an object called a `readtable`. We don't want to screw up the global readtable so we need our own copy.

```
    (let ((*readtable* (copy-readtable)))
      ...)
```

`*readtable*` is the global variable where the readtable lives, so now an use of the lisp function `read` inside the scope of the `let` will be using our `readtable` (this is effect is thread local).

Next we replace the `#+` & `#-` reader macros with our own function `my-new-reader-cond-func`:

```
    (set-dispatch-macro-character #\# #\+ my-new-reader-cond-func *readtable*)
    (set-dispatch-macro-character #\# #\- my-new-reader-cond-func *readtable*)
```

And that's it! `my-new-reader-cond-func` is actually a closure over an object that the conditions/results are cached into but that's just boring details.

The point is we can now introspect the reader conditions and know for certain what features were required in the spec file, and we do this without having to add any new practices for library writers.

This the reason for the TLDR at the top:

> If you add powerful features to your language you must be sure to balance them with equally powerful introspection

Or at least trust programmers with some of the internals so they can get things done. You can totally shoot your foot off with these features, but that ship sailed with macros anyway.

I wrapped all this up in a library you can find here: https://github.com/cbaggers/with-cached-reader-conditionals

### Other Stuff

Aside from this I:

- Pushed a whole bunch of code from master to release for `CEPL`, `Varjo`, `rtg-math` & `CEPL.SDL2`

- Requested that that new `with-cached-reader-conditionals` library and two others (for loading images) are added to quicklisp (the lisp package manager) So more code shipping! yay!


### Enough for now

Like I said, next week I'll have people over so I won't get much done, however my next goals are:

- Add groveler caching to the FFI

- Add functionality to the build system to copy dynamic libraries to the build directory of a compiled lisp program. This will need to handle the odd stuff OSX does with frameworks


Seeya folks, have a great week
