---
layout: post
title: TaleSpire Dev Log 378
description:
date: 2023-02-26 02:28:34
category:
tags: ['Bouncyrock', 'TaleSpire']
---

'Allo!

The latter half of the week has been 100% focused on Symbiotes.

In [my last post](https://bouncyrock.com/news/articles/talespire-dev-log-377) I talked about starting to make a code generator that would take a JSON spec for the API and generate the C# and JS plumbing code required for communicating between the WebView and TaleSpire. That is now working, and it's such a help.

It all but removes a class of user errors from the interconnect code as the boring stuff is generated. This also makes having slightly tighter but ugly encodings easier, as you don't need to update all the plumbing code directly. Also, by generating C# structs, we get type-checking, further reducing the number of ways for me to mess up[0].

Once the generator was working, I was able to write the implementations for about a dozen of the required API functions for the first version in a couple of hours. This really reassured me that it was worth the work.

After this, I merged the slab-repository branch into the Symbiotes one. This is because one of the Symbiote API functions allows you to get the slab string for the current selection, and some of the work on the slab branch made that much easier.

We then ran into an issue we knew was coming: sometimes, the user's JS can load in before the TaleSpire API has loaded. I added an option to the Symbiote manifest that allows the creator to specify a function that will be called on initialization and updated our setup code to support that.

With this all working, I got a build to Chairmander so he could continue his work, and I doubled back to the C# side of the Symbiotes feature. I spent a chunk of Friday cleaning up code so that all the JS and WebView-specific code lived together. As previously mentioned, I would like to see other kinds of Symbiote in the future, and the cleanup serves that potential future.

The next big todo is working on threading. I also need to get stuck back into slabs.

Anyhoo, that's enough rambling for now.

I hope you are having a lovely weekend.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] Just as well, I'm a master at breaking code :P
