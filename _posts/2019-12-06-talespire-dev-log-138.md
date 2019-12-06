---
layout: post
title: TaleSpire Dev Log 138
description:
date: 2019-12-06 09:09:52
category:
tags: ['TaleSpire', 'Bouncyrock']
---

I'm currently writing this on the train home,

I've been down working with Jonny for the last time (in person) this year, and it's been a great few days. We made a bunch of progress in different places, so lets natter about that.

### Tiles

First off, we were looking at tile placement and control. We may have found a nice change which could help with some tricky cases our alpha testers would get in when working with walls. My ultra vague language is due to it being so early in the prototype phase that we are not ready to talk about it yet. This is not the first time in the last 4 months when we have had a fix that play revealed to be worse than the initial problem. When we are more confident, we'll do a full writeup (or maybe a short video) to go through the different things (Just placing tiles has surprising nuances to it :D).

### Meatier News

As that is all arm waving and no substance, here is some real news: You will be able to place creature miniatures off-grid. This has been play-tested for a while internally now and, although it does introduce some challenges where it comes to UI/messaging, it does feel pretty cool. For those who have wrangled narrow corridors in the alpha, this should feel rather freeing :p

Do note that we are still keeping the tiles to the grid, however. Without this limitation, the worst-case complexity for various systems becomes totally unmanageable. Remember that doubling the length (or resolution) of the side of a cube increases the volume 8 times. So if you have anything that operates over the volume like say, fog of war or line of sight, your day would have gotten much worse (I know, I go on about this in almost every post).

### The Right Questions

I also think I know how cross-zone pathfinding will work now. I've been struggling a little with how to maintain a single graph as zones are loaded in and out. Jonny did me the simple favor of asking if it could be computed on demand. Building the nav-mesh from scratch each time a creature is picked up *sounds* like a lot of work, but it actually reduces the complexity a lot and opens up some opportunities for parallelization. We just need to keep the per-zone input data sorted in a way that helps this on-demand process as much as possible, and that bit is easier. More on this when I have tested it.

### Monster Branch Hunter

We have also started the long process of getting everything merged into master. The part of this that required us to work closely was moving to Unity assemblies for the core project as suddenly their 'magic folders' stop working[0]. This means reorganizing the whole codebase, which wasn't too bad[1]. However, this can make for seriously ugly merge conflicts and so it was best to stop development for a couple of hours, get the change merged in, and test on both our machines.

### Testing

I've also started working on another approach to ironing out bugs from the board code. It's very much inspired by my uninformed scanning of property-based testing. We make a dirt-simple but accurate model of the code we wish to test, and then we generate random streams of operations that are applied to both the real version and the model, and we compare the results. The model gets to ignore all details of multithreading, being performant, etc. Its only job is to be understandable and give the behavior and results intended from the real version. In previous tests, I had already made a fake network interconnect so I could apply operations to one board and make sure the other board ended up with exactly the same result after sync so now I can wire things up like this 

```
    [Randomly generated board operations]
             /              \
            |                |
            v                v
    [Simple Model]     [Real Board 0] <--fake network interconnect--> [Real Board 1] 
```

We feed the model and 'Real Board 0' the same operations, but then compare the results for all three boards. This gives us a pretty decent amount of testing for almost no additional work.

I will stress that this isn't property-based testing. I want to learn it, but I don't have the time to take on a new technique right now. However, I know that even this limited approach can give good results, and I think I'll be able to add some very basic test-case minimization too.

### Christmas Approaches

And with that, it's almost the end of the year. There is more to come, but I'm going to posting a bit less until January as I've managed to schedule everything to happen in one month, and apparently it's arrived. For the next week, I'm on holiday, and then I'll be heading to the UK for Christmas. During my time in the UK I'll be working some evenings on server stuff, so expect a very significant shift in content then!

I hope this finds you well folks.

Peace.



[0] by default, Unity automatically makes assemblies that separate game and editor code based on special folder names. It's handy but naturally goes away when you decide to handle assemblies yourself.

[1] There are a bunch of ways to layout your project, but we found it simplest to make 2 folders, 'Runtime' and 'Editors', and move all game code to the former and all the editor code to the latter. The directory structure on both sides is mirrored. We have a little helper menu for writing the boilerplate code for inspector editors, so we updated that to respect the new structure and added a right-click menu entry to jump to the editor folder if it existed.
