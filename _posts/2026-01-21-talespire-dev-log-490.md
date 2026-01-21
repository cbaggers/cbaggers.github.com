---
layout: post
title: TaleSpire Dev Log 490
description:
date: 2026-01-21 21:34:11
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

As promised, the minimally viable dev log!

I've continued work on the fixes post my major refactor of the particle tooling. Today has involved

- A bug in node deletion
- Fixing deserialize as it wasn't preserving node-inputs with fixed values (as opposed to a connection to another node)
- Improvements to the user api
- And assorted small fixes and improvements

Next up, I'm making it so removing a connection from an input automatically turns it into a value input (I'll show a gif tomorrow).

Then hopefully I'll probably be back to the runtime code.

Seeya tomorrow.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*
