---
layout: post
title: TaleSpire Dev Log 118
description:
date: 2019-07-16 00:07:04
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Another short dev log while we are still in the Kickstarter campaign.

Recently I've carried on studying. It looks like the hybrid renderer doesn't handle per-instance data yet (which is mad) so we won't be able to use that yet. This means we will probably want to avoid Unity's ECS for now and just work with something similar but custom until that stack matures. This is no real issue as although we will have to do a little bit of tooling work when we need insight we do have experience in that.

Ree has always been the lead dev when working with graphics and shaders in Unity as he has many years of experience beyond me. I (Baggers) come with a decent amount from the GL side though so I've spent some time getting more used to hlsl and tooling we've been using. It's fun, a lot of things are set up for you of course so I'm not sure (without the scriptable rendering pipeline) how one really packs data or does very custom stuff, but there's plenty of time to learn that and the team has bags of experience I can lean on. This is really just making sure we are cross skilled enough that we don't bottleneck each other too much (although when it comes to content creation I'm hopeless :D)

That's all for today,

Peace
