---
layout: post
title: TaleSpire Dev Log 409 - Jumping through hoops for Apple
description:
date: 2023-08-09 15:09:51
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

I'm still around and still pushing toward the macOS release. The last big technical hurdle before we can release is the digital equivalent of paperwork we must do for Apple to allow the game to run.

On Windows and Linux, you can (pretty much) just download programs and use them. Not so on Mac[0]. Apple requires that apps are "signed" and "notarized."

Roughly speaking, "signing" is a process that makes it easier to know who created an app and to validate that the contents of the app are unchanged. "Notarization" is where Apple scans the app for common vulnerabilities and checks that the app was signed correctly. Apple can then keep that information around so they can check that the app you have downloaded hasn't been messed with.

This wasn't always the case, but they've got stricter over the years, and now, even though we aren't using their App Store, we need to do these things.

So I've been trying to understand this process so I can add it to our build pipeline, and it has been a draaaag!

I started off looking into ways we could do the signing and notarizing from Windows (as our build agent is a Windows machine), and while there are some [interesting options](https://github.com/anchore/quill) we would have to read all their code to do a security review to make sure they aren't doing sketchy things with the certificates.

Given that we need to ship as soon as possible, we put that aside and looked into how to do this on Mac. We could buy another Mac, put it on one of our homes, and set up our build infrastructure talk to it. This makes us reliant on that person's power and internet working and would complicate things when they go on holiday. We could rent a Mac in a data center, but that is a non-trivial cost compared to other bare-metal servers. For now, we chose to use a [github action](https://github.com/features/actions) instead.

Github actions are, as always, just code running on someone else's computer, but it is quite convenient. We can choose to run on a Mac and then define a number of steps to run our scripts. We can also easily drop this if we decide to move to another approach. Borodust set all this up for us. With that, I just needed to make the shell script that would (finally) jump through Apple's hoops.

Now Apple documentation is a special beast. Whereas Microsoft documentation specializes in blunt unhelpfulness via tautology, Apple has seemingly assembled a team of lawyers and psyche-warfare specialists to remove anything that would allow too much certainty in your life. Apple forums are generally unhelpful, and due to being more of a walled garden, stackoverflow simply has fewer people posting Apple stuff.

It took me an embarrassingly long time to work out why a certificate I had installed and validated was not working (TLDR: it needed to be made from my signing request, not Ree's). I take some of the blame, but I could install the certificate, right-click on it and validate it for code-signing, and the Mac said, "Yup, this is good to go." But then, when trying to use it, the Mac would say, "Oh, I can't find that certificate."

I don't have the skill to explain how life-sapping debugging Apple shit is, but if you know an Apple dev, simply mention signing issues while looking in their eyes, and you'll see the hollow space where hope used to be.

ANYHOO, I finally got it going. If you ever need to do something similar, I've put our script [online here](https://gist.github.com/baggers-br/58f103f375778176d36a8d1025138d6e) so you can use it for inspiration [1].

I'm now testing it. Hopefully, this part is almost over.

Once it is, we should be able to give you a release date for Mac. My hope would be only a week or two later. That would give us time for a Beta and to deal with anything else that comes up.

Alright, enough of all that! I hope you are doing well or have something nice on the horizon.

Until next time,

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*


[0] Yeah, of course, you can change settings so that it only tells you it's not trusted and then forces you to go allow the program. But that isn't a good experience and is beyond the skill level of many casual computer users.

[1] At the time of this dev log going out, the script is still being tested. I've left a warning comment on the gist and will remove it when it works.
