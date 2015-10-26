---
layout: post
title: Events - Rewritten again
description:
date: 2015-10-26 10:39:18
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

Phew, now [Fuse](http://www.fusetools.com) has shipped the beta I'm not crunching so much at work so I'm back working on cepl again. (Check out Fuse btw, it's early days but theres some kickass shit in there, I'll be doing some posts on it in future)

I've not a nice branch full of changes to merge in soon in which I've been working on two main things:

1. Better error messages
This took a lot more work than just tweaking the strings. I've gone though varjo (the lisp->glsl compiler) trying to find places where I can make suggestions of possible correct alternatives. The obvious example for this is variable names so if you type the wrong name varjo will make suggestions based on names that are significantly similar (I'm just using jaro-winkler-distance for this).
In cepl it also involved going through the more hairy code, trying to seperate it into more sensible functions and also making sure I give more specific errors especially around the code that handles converting between texture-formats & lisp types.

2. New event system
So I changed this yet again :p. The reasons for doing this have mainly been around limiting scope. I dont want unnecesarily high level abstractions in cepl core, I want 'low enough' abstractions that are easy to build on.

So this time the main change is that event listeners are remvoed in favour of event-nodes.
Event nodes can subscribe to other event nodes to recieve their events. Any event node can also be subscribed to. Each node holds a list of weak refs to the node that are subscribed to it, and a list of strong refs to the nodes that it subscribes to. In this way we have a event graph that the garbage collector can clean up if you don't unsubscribe explicitly.

"Why add this weak ref complexity" would be a fair question to ask, and the reason is making a balance between good practice and pragmatism. Common Lisp allows awesome levels experimentation with the repl and incremental compiling. It is inevitable that, whilst explicitly disconnecting event-nodes is the correct thing to do, managing that is a PITA when you are just trying to play with ideas. To this end we make sure we can GC away parts of the event graph that are no longer needed, letting you focus on the important stuff. This kind of thing is what I mean by 'low level enough', I'm not trying to bake in some glitch free reactive programming framework (though those are awesome), but I still want to make the experience good and lispy.

Less importantly, with the new changes events are now structs with read only fields.


The driving force for the above changes has been that I now have event sources other than SDL2. I have made a simple android app that sends touch events over sockets to cepl to allow more direct interaction with whatever I'm making. It's early days but the events from the cepl remote-control are now flowing through the event system just like mouse & keyboard events so I'm pretty stoked :)

This physical hardware business makes recording youtube videos more tricky so I'm gonna go buy some extra cameras today so I can try show this stuff off.

Hopefully I'll be back soon with a new video for you.
Ciao
