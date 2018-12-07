---
layout: post
title: Response to 'Reasons why lisp games suffer'
description:
date: 2018-12-07 08:22:54
category:
tags: ['lisp', 'cepl']
---

I haven't been on `r/lisp` for a week or so due to being rather low on free time as we try and get an [alpha of our game](http://talespire.com) out of the door. When I did pop back I saw ['5 Reasons why Lisp Games Suffer and proposed solution'](https://terminal625.blogspot.com/2018/12/5-reasons-why-lisp-games-suffer-and.html) which I thought would be interesting, but what I found was bizzare and missinforming.

This article is, in my opinion, wrong from end to end so I'd like to go through it a bit. I realize this may come across as shitting on the authors intent which isn't my goal however being named explicitly in the article I feel like I can respond in some fashion. Also it'll sound like I'm shitting on lisp, which I dont feel I am.I adore working in this language but it is also just a piece of technology, just a tool, and we should be realistic about our tools when it comes to a given context.

### The article

Let's start with the first line

> Lisp games, like the Lisp AI and Lisp Machine hype in the 1980's, has proven yet again to fail to deliver.

This implies some massive lisp game hype akin to that of the lisp machines in the ai boom..so where is this hype? I've been around the lisp game community for a while now and none of the people who have been most active in making things are spouting anything like that line suggests. If there are people exposing lisp as some way to fix the game industry then they probably havent been making games in lisp. A lot goes into a game and decent programming languages are one part.


> Let's say I'm a lisp noob, or even an average gamer. I think, hey, what's the state of Lisp in games?

Ultimately gamers don't give a shit about your tech choices. If it runs well and is fun it could be TCL and MUMPS for all most people will care. Yes there are higher profile articles diving into the tech of games and these get a lot of followers but this doesnt mean the majority are peering into the internals and judging games based on this.

__sidenote:__ *for those interested in this stuff I highly recommend [Adrian Courrèges](http://www.adriancourreges.com/blog/2015/11/02/gta-v-graphics-study/) articles*


> The Poverty of #Lispgames TLDR: Lisp games has many channels, but not enough content

I'm not sure what 'not enough' means in this case, not enough for what? for who? If it's not enough to satisfy the fallacious first line of the article then there is no doubt that this will never be satisfied.

Setting up channels for sharing content is a cheap endeavor compared to making the content. People are having fun, they enjoy lisp and they want to share stuff and big platforms make that easy, I'm not sure that holding off making those channels until the community reaches some assumed level of success makes any sense. Unless your goals and assumptions are rather out of sync with what those making these channel's seems to be.


> But where are the games?

SURPRISE! games are hard and just having a nice language doesn't make that go away.

Also a lot of us enjoy working on problems, and games afford us a lot of interesting limitations which allow us to stretch our minds by finding solutions that would fit in this language we love.

CEPL is a prime example of this. CEPL doesn't need to exist at all, it's purely my attempt to make a high level api with which I could *play* with GL and yet which doesn't hide any of GL's features. This is tricky, fun problem as I feel that many apis I have worked with provide simplicity by hiding things and I don't want that.

If my drive was to make a specific game I would shut up and make that game... BUT, and this is an important bit, if your goal is to make a specific game then you should be using the best tools available to make that game, and that likely is not going to include lisp.

That might sound odd given my background but please bear with me, I'll rant a bit more about this below. Let's wade through the rest of this thing first.


> Outnumbered by Blub

This section is just a bunch of BS based on false assumptions.

Lisp is a niche language, and making games is a niche within it. Golly gosh I wonder why there isnt much being made.

Even withing that sub-niche there is a significant percentage of people who just want to make tools, not games, so there goes a few more potential games.

Even the use of the term 'blub' pisses me off here. When we talk about making a game we have left the realm of being academic about a language and have entered the world of making a product. That product has to run on real devices used by real people. Those devices have actual hardware that have actual characteristic and so a language's ability to leverage those devices is what dictates how good it can be for a given task. When you speak of non lisps are 'blub' you disregard the actual characteristic of those languages and how they fit to the task.

There are many folks in the game industry who dont like c++, but it's still the number one language used for 2 core reasons (there are more but for my narrative 2 will suffice):

1. Because it remains one of the few major languages to give the kind of control over data and instruction layout that is *mandatory* if you want to ship a high end game on all platforms.

2. Momentum. There is a lot of existing libraries written in C++ and C++ is difficult to interface with efficiently from other languages. And yes I know about `extern "C"` but try taking the [bullet physics library](https://github.com/bulletphysics/bullet3) or even [the fbx sdk](https://www.autodesk.com/developer-network/platform-technologies/fbx-sdk-2019-0) sdk and expose those with a sane C api. It's a monster of a task and just not worth it for studios with the kinds of time and money budgets they have.


> The Sorry Tale of Those Who Tread the Great Lisp Path

The entitlement is this section is strong.

I don't know how someone could call out a game creator by saying 'So much for sticking to common lisp' with a straight face. They don't owe you, or lisp, anything. Lisp is a tool.

Same for warweasel's work on Clinch. Clinch had crazily large ambitions and it didn't work out. That happens, and it's fine. Now he's got a specific game he wants to work on and so has picked a tool to make that feasible, that is a good move.

Then there is just random bitching that they don't like Shinmera's video style. Not sure what that has to do with anything, however as a regular viewer I liked them. The world has room for long and shortform videos.

As someone who [does a little of that myself](https://www.youtube.com/user/CBaggers/videos) I can tell you that my 5 minutes videos can often take longer to make than my 2 hour videos due to the number of takes and tweaks required to make sure the content hits all of the beats it needs to in such a short time slot. Even putting that aside, somehow equating this to a reason that lisp games are failing is a seriously warped outlook.

Next let's take a quick detour before I get to the bit that really raised my eyebrows

> Difficult to install, difficult to run, difficult to mod, difficult to use

Nothing here is in any way limited to games in lisp, and the whole 'modding' aspect of it doesn't make any sense. I'll spare you a diatribe on this unless it becomes necessary.

And Finally!

> Is Baggers leaving?

Fuck off.

Why not just contact me if that was something so noteworthy you think it belongs in here. Although I'm pretty slow to reply to emails I do get to them. I'm also always logged into the lisp discord channel and am DM'able on twitter. In short there are a pile of ways to reach me and lack of even an attempt to do that is lazy at best.

I'd mind less if this way an off the cuff twitter comment, but this is an article purporting to shed light on some issue and provide some answers. Even the minimum amount of work would have answered your question. I even explained I wasn't leaving in many of my streams (not that I expect people to be watching them, but if you make claims a minimum of due diligence wouldn't hurt).

This whole article reeks of this kind of laziness, it points the finger at all the wrong things whilst missing everything that does stop me using lisp for serious games today.

For what it's worth, I'm currently working my ass off to try and get a game out the door. I've saved up enough money that I could quit my job and try this but that stash wont last for long. So fuck yeah I'm going to pour myself into that work, fuck yeah I'm gonna work my hardest to see this thing succeed and fuck you for calling me out just because I haven't given you significant free work in 4 months.

### Actual thoughts on lisp's applicability for games

Phew, with that out of the way I'll try and speak a little to the original question. Why not lisp?

I am not going to make any bigger claims on the community or industry as a whole so this is just where I'm at right now. I'll also touch a little on why I'm not using lisp for my current games


#### 1. Performance matters

You need to hit your target fps, whether that is 30 or 60 but it's usually the latter. A game that runs too slow or that inconsistent performance doesn't just stop being fun, it stops being a game. Of course many games dont push the machine that hard, but we are talking about games as a whole and a vast number do.

We also need to run on mobile devices which don't have the grunt your desktop gaming rig does.

Performance (beyond the efficiency gained from appropriate algorithms) is going to be dictated from your ability to control what is being processed and when. Maybe I can't afford a GC in the middle of my frame, maybe to get the entity count I need I really have to maintain data locality.

ANSI CL doesn't have many provisions to allow for this kind of control. Certain implementations do have some and you can do a lot with CFFI, but at what point do the restrictions you impose on yourself making another language a better candidate?

There is a lot of room for lisp to do good things in this space and I'm looking forward to working on some things in this area but for those who just want write a game today.. maybe it's not for them.


#### 2. Games of any real size are rarely single person endeavors

Making a game from scratch is a lot of work. Even with libraries and engines (see point 2) you are likely to be collaborating with people. Given a limited budget this means you all need to be work at your peak, so trade-offs will be made to optimize for the team as a whole.

Yeah I'd love to code in lisp every day. But (even putting aside lisp not running everywhere I need it) i'd be throwing away 13 years of experience of another member of my team. There are 3 of us and so I just kneecapped a third of my team for what?


#### 3. Libraries and Engines

This is really a continuation of the last point but when you use an engine or library you lean on the work of hundred or thousands of other developers. Have a peek at [this changelog](https://www.unrealengine.com/en-US/blog/unreal-engine-4-21-released) for ONE sub-version of Unreal, then think about the tens or hundreds of man-years on work that has gone into that one version.

We have less of these libraries in lisp and so a lisp game developer today is going to need to do far more work than one using Unity (for example) due being forced to create more from scratch.


#### 4. High level api design is a fickle beast

In lisp it's very easy to make high level apis and DSL over systems. However how relevant a particular solution is to a given person varies wildly. It's not just aesthetic either, the features of the language we choose have performance implications and not every game can afford to make the choices you can afford for yours. Generic methods are an example here, a great feature but everything has a cost.

In my opinion the approach of providing low level bindings over C libraries separately from the high level api is a way better way of fostering code reuse in lisp (borodust is a shining example of this).


#### 5. Portability matters

I need to get code on mobile platforms & consoles. Lisp doesn't help me here. I could shell out a shit tonne of money for lispworks or dabble with the relatively unknown mocl. But that only gets me mobile and it's still only the language, I need to port all the tools and many of my C dependencies probably dont work on the PS4.

The alternative is I can use Unity or Unreal for free and use code that was tuned for games on those platforms today.


#### 6. Content is king

The content pipeline is staggeringly absent from the provoking article. The ability for you to consume models, video, images etc from the swath of authoring tools out there is a serious challenge and having [assimp](http://www.assimp.org/) and [sdl_image](https://www.libsdl.org/projects/SDL_image/) is not nearly enough.

Many of the libraries you will need are C++ and not super amenable to being wrapped. Even for the ones that are that work still needs to be done and be maintained.

Remember that you are a member of a team and telling your artist to just use blender and obj is not an option.

With unity I can drag in FBX files (in the latest format) and I get what I expect. And the representation of the data in memory is being handled based on how I'm handling and rendering those objects.


### Something a little more upbeat

CL is a great language that still has a lot to give. I'm gonna be using it for a long time including for games. It lives in the place of being fantastic and performant for scripting and also having the control to be truly viable for higher performance code. The implantation specific extensions to the language open up a ton of possibilities for squeezing more out of the machine and the abilities that macros afford us means there is a lot of meaningful stuff we can do trivially that require complex and unwieldy systems in other languages.

For the right project and with the right support, CL can be a great tool in your journey to make a game. But it makes perfect sense that it isn't that for everyone right now. And it's ok if it doesn't ever become that.

I'll be back to lisping early next year and I'd love to natter more about these things but this post was too long a thousand words ago.

### To the original author

I'd heartily recommend making a non-trivial game in lisp. Like something that was mainstream 10 years ago and try and remake it. The problems you run into will tell you so much and your fixes (if you choose to share them) will help the community in measurable ways.

Personally I'd recommend halflife 2. It's 14 years old now but you should be able to find rips of the assets which removes that whole part of the equation.

### Enough, lets wrap this up

To close I'd like to say the following:

If you'd like to make things, I'd advise trying to gain and understanding of yourself and what you need. What is driving you, what do you feel incomplete without. If you *need* a specific game to exist then ignore the pseudo-religious posturings of various technology advocates and just focus on making it a reality. Use the best tools for the job and get it out there. I'll be rooting for you.
