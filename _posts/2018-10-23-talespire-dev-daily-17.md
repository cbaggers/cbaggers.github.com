---
layout: post
title: TaleSpire Dev Daily 17
description:
date: 2018-10-22 23:56:46
category:
tags: ['BouncyRock', 'TaleSpire']
---

Hi again, today has been a day of churn. We have known for a couple of weeks that the way floors have worked in the prototype was unsatisfactory. For context you used to be able to delete floors and insert floors below others, which sounds great and felt great to.. on small boards. However when you start making a whole village and want to add a floor to a single building then the problems quickly become apparent, you add a floor to the entire village!

So instead of that we will be replacing it with the ability to select a region and:

- delete it
- delete it and drop the tiles from floors above down
- push tiles up to the next floor (which pushes the ones above up etc)

This will give the level of control we had before but without the caveats. Of course this requires a little more work from the user, but it feels like a necessary inconvenience.

Also, in order to be able to support large board in the future we needed to get away from anything that operated on entire floors so chunking up the board has been on the todo list for a while (we already had started this for the fog of war code). This is where the churn came in as, as you can imagine, floors were pretty central in the codebase so changing that out has touched a tonne of code. Getting things to behave again after those changes took most of today but it's creeping toward feeling solid again.

I'm not 100% sure what I'm working on tomorrow. I still have a lot of tidy-up work to do, I need to look into how to add 'hill climbing' to the octree fill (more on that another day) and I need to make some server-side changes very soon.

Guess I'll see how I feel in the morning.

For now have a wee video showing the fog of war responding to the door opening. I have to switch between DM and player here as attempting to open the door will try sending a message to the DM.. who is on the same machine and the UI code gets a bit confused :)

Seeya


<iframe width="560" height="315" src="https://www.youtube.com/embed/54uEaKz0aU8" frameborder="0"></iframe>
