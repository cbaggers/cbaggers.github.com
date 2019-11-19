---
layout: post
title: TaleSpire Dev Log 130
description:
date: 2019-11-19 23:06:10
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today I carried on working on the scripting implementation.

Each script has a small section of memory that it will be able to store values to, my first job of the day was finishing of that data store. A few things were a little difficult to debug as VS2017's debugger doesn't show what data is on the other end of a pointer.

After finishing I found out that VS2019 does have this feature so I spent a couple of hours fighting to get that upgraded and playing with Unity. If you try this yourself remember:

- Don't let VS install unity again, the version is old. Search for 'Unity' in the components section of the installer and find the tools "unity tools for visual studio" component
- Open Unity's package manager and make sure "Visual Studio Code Editor" is up to date
- If VS2019 isn't appearing in `Preferences -> External Tools -> External Script Editor` then click browse and find something similar to `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`
- If like me you only use VS for debugging and have a different editor for programming you will need to update omnisharp (or your equivalent) that and probably your editor integration too.
- Restart everything :p Just for safety

After all this, I hooked up the scripting system so that the board asset arrays now receive the references into Spaghet's data store. All is working according to plan there.

The next thing I want to do is look at moving Spaghet into its own package so I can share that between TaleWeaver (the asset prep and modding tool) and TaleSpire itself.

That's gonna take a little coordination with the rest of the team so during that I'm going to have a look at making the Unity component that lets you connect resources to your scripts. Here is the old (very unfinished) one that held the LUA scripts.

![script assets](/assets/images/scriptAttribs0.png)

The important part, for now, is the bit marked in red. You need to be able to add resource slots so you can drag in the animator or sound effect you are interested in. Then those resources can be referred to from the script.

Another potentially nicer way to handle it would be have the slots on the state machine nodes themselves.. hmm actually there might be a nice way to do that as the nodes are made with Unity's imgui elements. I'll look into that tomorrow.

Whatever approach I use for beta, I'll then need to look for this element when the asset is first loaded in TaleSpire and submit the scripts to Spaghet.

Slowly slowly all these threads are weaving together.

More tomorrow.

Ciao
