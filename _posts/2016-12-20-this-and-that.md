---
layout: post
title: This and That
description:
date: 2016-12-20 11:23:30
category:
tags: ['lisp', 'cepl', 'varjo', 'pbr']
commentIssueId:
---

I seem to oscillate slowly between needing to create and needed to consume media. This week I've either jumped back to consume or I'm procrastinating.

Either way I've not been super productive, let's start with the consumption:

### Nom nom knowledge

- Listened to [Iceberg Slim's biography](https://www.youtube.com/playlist?list=PLSRtWqmC79Ka5YqNJeHP6gbzLTVtWrqh_) - On stage one time Dave Chappele was talking about how the media industry worked and likened it to pimping. He said this book pretty much explained the system. It's damn depressing.

- Watched some TED talks. Filtering out all the TEDX shit really makes the site more valuable
 - [craig venter unveils synthetic life](http://www.ted.com/talks/craig_venter_unveils_synthetic_life#t-848430) - It's now been 5 years since the first man-made lifeform was booted-up. Fucking incredible work. Everything this guy works on is worth your time
 - [your brain on communication](http://www.ted.com/talks/uri_hasson_this_is_your_brain_on_communication) - FMRIs taken on people being read stories. Very cool way of setting up the tests to reveal the layered nature of brain processing
 - [scientist_makes_ears_out_of_apples](http://www.ted.com/talks/andrew_pelling_this_scientist_makes_ears_out_of_apples#t-262467) - less on the bleeding edge but very cool. Seems that stripping living things down totheir cellulose structure is possible in a home lab. I'd like to mess with this some day.
 - [how this fbi strategy is actually creating us based terrorists](http://www.ted.com/talks/trevor_aaronson_how_this_fbi_strategy_is_actually_creating_us_based_terrorists) - Fucking infuriating but relieving to see how much info is available through the court proceedings
 - [The surprisingly logic minds of babies](http://www.ted.com/talks/laura_schulz_the_surprisingly_logical_minds_of_babies#t-1202930) - I loved this one. Babies can infer based on probabilities. Very cool to see how these tests are constructed and good food for thought when musing on how brains work.
 - [What really matters at the end of life](http://www.ted.com/talks/bj_miller_what_really_matters_at_the_end_of_life#t-925839) - Watch this. Is about designing for dying. Very moving, very important, I wish I could say all my family could leave on the terms he describes. Also badass is the fact that the talk is given by a cyborg and that isnt the point of the talk. I love this part of our future.
 - [The sore problem of prosthetic limbs](http://www.ted.com/talks/david_sengeh_the_sore_problem_of_prosthetic_limbs) - How to make comfortable sockets for prosthetic limbs. I'm fairly sure this can be done without the MRI step, which would make it much cheaper and more portable. I'll have to look into this at some point
 - [How to read the genome and build a human being](http://www.ted.com/talks/riccardo_sabatini_how_to_read_the_genome_and_build_a_human_being#t-802703) - Really cool to see how machine analysis of the geneome can let us predict some characteristics with high accuracy (height to within 5cm for example). A good intro talk
 - [Adam Savage's love letter to cosplay](http://www.ted.com/talks/adam_savage_my_love_letter_to_cosplay#t-545334) - Passion and community, I loved this talk.

- I've also been listening to a book called ['The Information: A History, a Theory, a Flood'](https://en.wikipedia.org/wiki/The_Information:_A_History,_a_Theory,_a_Flood) again. It's an outstanding walk through our history of understand what information is. From african drums, to morse code, to computers, to genes. This book rules. Read/Listen to it. I've been repeating 2 chapters this last week trying to bake them into my brain. The first was on entropy (and how information & entropy are the same), and the second is on genes and how information flows from and through us.

### Making stuffs

The first order of business was to look at PBR. Previously I had got deferred point lights to work, however I failed hard at the IBL step. Luckily I rediscovered [this tutorial](http://www.codinglabs.net/article_physically_based_rendering.aspx) as understood how it fit into what I was doing. Last time I had tried to stick to one paper as (in my ignorance) each approach felt very different to me.

I wrote the shader in lisp and immediately ran into a few bugs in my compiler. This sucked and the fixes I made werent satisfying. The problems were dumb side-effects of how i was doing my flow analysis, I'm pretty sure now that I want to get my `compiler-time-values` feature finished and then rewrite the code that uses the flow analyzer to use that instead.

I then ran into a few rendering issues. The first turned out to be a bug in my implementation of a function that samples from a cross texture (commonly used for environment maps). The next 2 I haven't fixed yet:

- the env map filtering pass is super slow
- the resulting env map has horrifying banding

I checked the generated glsl and it looks fine. I'm struggling to work out how I'm screwing this up. I guess it could be that I have a bug in how I'm binding/unbinding textures and this is causing a flush..that could account for the speed...and maybe the graphical fuckups? I don't know man.

Despite that it feels good to be back in that codebase. One thing that really stood out though was how much first-class functions could make the codebase cleaner and more flexible. I had started that feature partially for fun but more and more it seems it's going to be very useful.

Given that I spent last night digging into that branch of my compiler. I decided that even without support for closures it would still be a good feature. So I did the following:

- Throw exception on attempts to pass a closure as a value. I've tried to make the error message friendly so people get what is happening
- fixed a couple of glsl generation bugs around passing functions as objects

I then spent a little time looking into how to generalize my compile-time-value feature, this will mean I can not only pass around functions but values user defined types. I'm going to use this for vector spaces. I realized that this doesn't currently have enough power to cover all the things I could do with flow analysis, this is a bummer but at this point I had drunk too much wine to come up with good ideas so I called it a night.

Next week I need to crack the new version of the spaces feature, get that merge in and get back into the PBR.

.. Oh and Christmas :p
