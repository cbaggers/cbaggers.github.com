---
layout: post
title: TaleSpire Dev Log 492
description:
date: 2026-01-25 21:56:21
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Friday went well enough. I was preparing to rewrite the editor code that runs the effects, as I need to make it work outside of play-mode.

This was a good time to restructure the project, so I was doing that and experimenting with what Unity allows it terms of spreading the definitions of scriptable objects over multiple assemblies. The answer is, in short, it doesn't. The C# compiler is happy enough with it, but Unity’s runtime is not. It’s fine though, it was simply an effort to keep the codebase cleaner by separating the code that is needed at runtime from the parts that are not.

Tomorrow I’ll be getting stuck into getting particles back on screen, and exploring [ExecuteAlways](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/ExecuteAlways.html), which is something I’ve only used for very simple systems before now.

Seeya in the next dev-log

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn’t reflect everything going on with the team*
