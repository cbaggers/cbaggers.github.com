---
layout: post
title: Entity Systems in Games
description:
category:
tags: ['game engines', 'entity systems']
---

As I have been making good headway on studying OpenGL I have been starting to have a look into more of the things I will need to make real games using my tools. 

One of the obvious first steps is working out how I will manage all the different types of 'things' (lights, boxes, triggers, explosions, hats..and err..BIG HATS) in the games and also how to defining and extending their functionality as easy and powerful as possible.

It doesnt take much googling to see that object-orientated programming is not actually the way to go. This seems strange at first as OO can seem a natural fit for defining types of object. I mean you can have something like this:

    character
      |
      |__human
      |   |___soldier
      |   |___civilian
      |
      |__alien
           |____alien queen
           |____alien soldier

But it turns out that in real life, things change (who knew?!) and, as new type of object are need in the game, functionality that use to be specific to one class of game object has to be moved up the tree.

For example, lets look at out pretend object hierarchy above. When the game designers first added the 'alien solider' they wanted to let it be able to call re-enforcements, this is fine but the 'human->solider" also has this functionality. As good coders we want to reduce code duplication but where do we put the 'call re-enforcements' functionality? The only shared class is the 'character' class so we would have to put it there...but now this means that the 'civilian' and the 'alien queen' also inherit 'call re-enforcements' even though they will never do this in-game.

It turns out that this kind of thing happens a lot, and you start getting classes (like 'character' here) which become dumping grounds for functionality. These classes are often called Blobs.

Ok so if this isn't what we want then what other options do we have?

Well the one that is interesting me is called an Entity System. It detatches the functionality from the data which allows for composition of game objects in a way that is much more flexible than multiple inheritance ...well rather than me doing a poor job replicating the articles I have read I will just link them below, hopefully they will help you as much as they have me.

* [Entity Systems are the future of MMOG development](http://t-machine.org/index.php/2007/09/03/entity-systems-are-the-future-of-mmog-development-part-1/) This is a series of posts on the Entity Sytems, give yourself some time to read throught these.
* [Evolve Your Hierarchy - Refactoring Game Entities with Components](http://t-machine.org/index.php/2007/09/03/entity-systems-are-the-future-of-mmog-development-part-1/) This has a narrower focus but good explanations, it was the first article I read on the subject.
* [Role of systems in entity systems architecture](http://gamedev.stackexchange.com/questions/31473/role-of-systems-in-entity-systems-architecture) If the above have thrown you a little this is a brilliant breakdown of entity systems.

Right I'm very tired now, Goodnight Everyone!
