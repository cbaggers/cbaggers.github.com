---
layout: post
title: TaleSpire Dev Log 426
description:
date: 2024-01-08 23:29:28
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks, I hope the first week of the year is treating you well.

Things are going well over here. I feel well back into the swing of things now, so it is a good time for an update on what I'm doing.

Before Christmas, I had been pushing hard to get creature modding shipped, but clearly, I didn't succeed. I thought I could hack around some of the tricky cases and get something made to see us through the holidays. If that had worked, my January would have been rewriting all the hacks into reasonable code.

As I failed to hit that target, I felt I had a couple of options.

1. Continue the crunch and try and get the hacks to work. And then immediately rewrite them all.
2. Just bite the bullet and do it more correctly, with the downside being that it would take more of January to do it.

I chose option two.

I chose that as my personal goal this year is to get better balance and organization in my life, something I have always struggled with. If I'm in doubt[0], I always find it easier just to work as I know what I'm doing with code as opposed to... well, everything else. But you can only do that for so long before cracks appear elsewhere.

This choice is feeling good for me so far.

That's quite enough of the human meat issues; let's talk technical stuff instead!

## Content Addresses

Until now, most of the content you interact with in TaleSpire has been identified by a UUID. This gives us a (hopefully) globally unique identifier (name) for each creature, tile, prop, etc.

When you copy a slab, those IDs are included in the weird string that TaleSpire produces, and when you paste it back in, we use the IDs to find the data for the tiles/props/etc that you are pasting.

Now imagine how it will be with mods. A person may copy a slab with tiles or props made by the community or (soon) may be hosted on a repository that Bouncyrock does not control. How can TaleSpire find the data it needs only using the UUID from before?

There are a bunch of possibilities. One is that we could host a central address book that lets TaleSpire look up an ID and find the content pack and repository that contains it. This, however, means registering all content and IDs with use, centralizing information, and giving us too much power over creators. Instead, we are borrowing from the web and moving to URIs.

With the new scheme, every piece of content has a content-address:

Here is an example one: `:46be6663d03843b6bb320a97716e5ace#72fbaa19-806d-4816-8d7f-91f17a459b93`

Without the scheme, this identifies a piece of content named `72fbaa19-806d-4816-8d7f-91f17a459b93` in the internal content-pack `46be6663d03843b6bb320a97716e5ace` (An internal content-pack is one that is shipped as part of the game)

The `#` is the start of a [fragment](https://en.wikipedia.org/wiki/URI_fragment), often used in URLs to point to some content within whatever thing the main part of the URL was pointing to. That's precisely what we are doing, too.

Here is a different one: `repo://talesbazaar.com/api/7d745de08e494e7ea7dce871519c09f0#d0bd95b7-0ef3-4144-a1ee-63aeedc93707`

This doesn't work yet, as we haven't yet published the community repository REST API specification. But, once we do, a site like [talestavern.com](https://talestavern.com/) or [talesbazaar](https://talesbazaar.com) could implement it, and then TaleSpire would be able to fetch content from places that are entirely out of our hands (We think this is critical for the longevity of TaleSpire).

- The `repo:` scheme tells us it is a public repository.
- The `talesbazaar.com/api/` portion tells us the location of the endpoint (`https` is always used, so that doesn't need to be specified in our URI).
- The `7d745de08e494e7ea7dce871519c09f0` is a hex-encoded content-pack ID. It's not globally unique, but it will uniquely identify a pack in the repository
- Once again, the fragment `d0bd95b7-0ef3-4144-a1ee-63aeedc93707` is the UUID of the content inside the pack we are fetching.

We have other schemes for mod.io, HeroForge, and we are even tinkering with fetching content from arbitrary web addresses. This is proving useful for icons that are hosted separately from the content itself.

This is just example syntax, but I'm experimenting with URIs like this: `web://some-site.com/some-place/some-image.png#64,64,32,32`

In this case, the "content-pack" is an image at `https://some-site.com/some-place/some-image.png`, and the fragment identifies a region of the image that is then used as an icon.

## Async everywhere

TaleSpire grew out of a simple, single-floor, dungeon-building, TTRPG-esque game. Initially, all of its content was built in, and there is still code that assumes some resources are immediately available. Over the years, we've been moving away from that as we tackled HeroForge and the community mod browser, but there is plenty of old code around.

As you can imagine, modding breaks all of that!

I won't be able to fix all these old assumptions this time. I will leave some for when we add modding for Tiles and Props, but still, it's a lot of work.

Each time I finish a feature like the mod browser, I think, "Ah, yeah, now we've got all the bits we need to make the real solution," and each time, something else comes up. I'm sure there will be something else I need to handle. I'll be sure to let you know when it happens!

## Progress

All in all, though, it's going well. In a game that is so much about displaying and navigating content, it's no surprise that changing the fundamentals of how content is addressed, obtained, and loaded touches a significant portion of the project.

I'm working through hundreds of files and tens of thousands of lines of code and doing so carefully, as I never want to break anything you've been building.

I think I'll be through the first wave of TaleSpire refactorings in a few days, and then I'll move on to TaleWeaver, where there is a TON of work to do as the internal content-pack formats have completely changed.

I'm very optimistic, though, and I'll keep you posted as it all quickly gets closer to release.

Man, this got so long, and I haven't even talked about the ways we intern the URIs to make them fast to work with at runtime or any of the other stuff we are doing, but that will have to keep for another day.

Have a great night folks,

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] and I'm always in doubt :P
