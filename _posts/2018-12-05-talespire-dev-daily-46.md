---
layout: post
title: TaleSpire Dev Daily 46
description:
date: 2018-12-05 00:18:09
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Evening all! Today has seen a bunch of bug fixes which are rather specific but I'll mention a few.

- Changing floor rapidly on board load could cause assets not to load
- Deleting the board you are in now takes you to the campaigns default board (currently a new empty board)
- Fix for case some requested data arrives for a board asset which has been deleted
- Grabbing a board asset whilst board loading caused error as creature landed in invalid area
 - This is half the fix, the other half is a UI fix to make sure you can't pick things up before the zone is fully loaded
- Add inspector button for checking for duplicate network ids in board assets
- Fix case which resulted in duplicate network ids after reloading board
- Add compression to network messages for building/deleting multiple tiles
- batch multi-tile messages to avoid making single message too large
 - not as robust as I'd like but good enough for alpha
- Fix case where other player deleting assets we have selected caused our selected region to grow
- A bunch of code cleanups

I also hit an annoying aspect of mono. In c# you are able to set the [validator](https://docs.microsoft.com/en-us/dotnet/api/system.net.security.remotecertificatevalidationcallback?view=netframework-4.7.2) used when a HttpWebRequest needs to check if a http request is valid. One of the things that is meant to be passed to that callback is the certificate chain. I use this to verify the intermediates on top of our certificate being checked. On MS c# this works perfectly, but in the mono version unity is using it isn't. Super aggravating but not much we can do right now, hopefully it will improve as the .net version support in Unity gets better.

The standard certificate validation is fine of course so no worries on the security side. I just wanted the option to do a bit extra.

Anyhoo that's all for today. Tomorrow I am looking at managing the connection to the backend and the handling/retrying/etc of failed requests.

Peace.
