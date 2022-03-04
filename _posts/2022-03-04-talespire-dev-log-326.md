---
layout: post
title: TaleSpire Dev Log 326
description:
date: 2022-03-03 23:35:01
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey everyone.

Today has not wanted me to make progress!

I have a few working days before the new HeroForge asset changes are available, so I thought I'd look at other upcoming features. So I:

- reviewed the roadmap
- reread the server chat code
- got an overview of rulers
- came up with more emote ideas
- made some more notes for some potential gm tool for previewing what you players can see

But these are just notes. I wanted to dig into something. I did some reading around Metal (Apple's graphics API), specifically how to read back data from textures. It looked fun, so I thought I'd dig into Mac support.

So I:
- updated the Unity version
- worked around a new bug
- pulled the latest version of Steamworks.NET

...and then promptly ran into a bug where https requests weren't working. It's looking like [HttpClientHandler.ServerCertificateCustomValidationCallback](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback?view=netframework-4.7.2) is not working, and so certificate pinning is broken.

This is maddening, and I've spent a good few hours poking around code to see if another code path would work. So far, it seems not.

This means I should probably roll back to earlier Unity builds to see if they work. However, while this was going wrong, I found some server-side work requiring prompt attention. So it looks like that's where my time is going next.

Yeah, so it was a bit of an annoying day. I was pumped to make stuff, and I ran into walls instead.

No worries, though. That's just how it goes some days.

Have a good one.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
