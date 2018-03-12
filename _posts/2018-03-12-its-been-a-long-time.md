---
layout: post
title: It's been a long time
description:
date: 2018-03-12 12:45:12
category:
ptags: ['lisp', 'varjo']
commentIssueId:
---

> .. how have you been?
> - glados

I fell out of the habit of writing these over christmas so it's reall time for me to start this up again.

Currently I am reading ________ in preparation for my new job but I have also had some time for lisp.

On the lisp side I have been giving a bit more time to Varjo as there is a lot of completeness & stability things I need to work on over there. I've hacked a few extra sanity checks for qualifiers, however the checks are scattered around and it feels sucky so I really need to do a cleanup pass.

In good news I've also made a sweep through the CL spec looking for things that are worth adding to Vari. This resulted in [this list](https://github.com/cbaggers/varjo/issues/168#issuecomment-371074348) and I've been able to add whole bunch of things. This really helps Vari feel closer to Common Lisp.

One interesting issue though is around clashes in the spec; lets take `round` for example. In CL `round` rounds to the nearest even, in GLSL it rounds in an implementation defined direction. GLSL does provide `roundEven` which more closely matches CL's behavior. So the conundrum is, do we defined Vari's `round` using `roundEven` or `round`. One way we may trip up CL programmers, the other way we trip up GLSL programmers, and also anyone porting code from GLSL to Vari without being aware of those rules. We can of course provide a `fast-round` function, but this doesnt neccessarily help with the issue of discoverability or 'least surprise'.

I'm leaning towards keeping the GLSL meaning however, simply as we are already not writing true CL and our dialect makes sacrifices in the name of performance already. Maybe this is ok.

That's all I've got for now, seeya all next week. 

p.s. No stream this week as [ferris](https://www.twitch.tv/ferrisstreamsstuff) is giving a [rust talk](https://www.meetup.com/Rust-Oslo/events/247872474/) here in Oslo.

p.p.s I also feel the 'I haven't been making lil-bits-of-lisp videos in ages' guilt so I need to get back to that soon.
