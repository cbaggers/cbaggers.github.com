---
layout: post
title: TaleSpire Dev Log 325
description:
date: 2022-02-28 19:53:17
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hey folks,

How the hell has it been a week since the last one of these?! Let's remedy that.

#### AssetDb

The `AssetDb` is a chunk of code that handles the loading of assets and indexes. It keeps track of metadata such as tags and groups and provides hooks for other systems.

Up until now, we haven't needed to unload packages at runtime. With modding, this all changes, so I've been adding that functionality.

There were a few false starts and dead-ends as I tried to find a balance between simplicity and speed. I'm still trying to learn the skill of choosing the approach that is just dumb enough to work while still being amenable to development over time. Getting stuff working faster and iterating is so valuable.

Asset code obviously touches many things in TaleSpire, so you find yourself in bits of the codebase that are quite old (like the inventory bar at the bottom). Once again, we don't want to spend too much effort on these things as we already have proper improvements planned for the future. So I find myself holding off any *smart* fix that would be too invasive.

All that rambling aside, it's going well. Unloading is working great. Next up will be testing and fixing systems that don't know how to react to assets becoming unavailable dynamically.


#### Fix tesla-coil & blue-fire

We have had an intermittent bug with tesla-coils occasionally not showing their lightning for a long time now. It's always been tricky to track down as I didn't have a board that could reliably reproduce the issue. Every time I would test, the damn thing would work, so the ticket would keep falling further back in the bug list.

However, last week the aberration pack shipped, and we had reports of the blue fire asset not working for some too. Ree quickly managed to track down one issue[0], but the fire behaved suspiciously like the tesla-coils even with that patched.

Unlike previous efforts, this time, we got lucky. One of our moderators had a board where they could semi-reliably get the tesla-coil bug. She published it, passed me the link, and we were off!

I was able to find out the following:

- If I imported the board and then placed a coil, it would fail
- If I reloaded the board, the previously placed coil and all new ones would work

This is precisely the kind of place I love to start, and I quickly tried to delete parts of the board until I could get the smallest number of tiles that triggered the issue. This can yield a minimal test case, but this time it was very stubborn. If I removed any significant chunk of the board, the tesla coils just started working again.

That in itself is data, of course, so I changed tactic and started stepping through the batching code in the debugger.

I should take a tiny diversion to explain that the commonality between the coils and fire is that they are both 'multi-mesh' animations. To get the stop-motion-like effect of the flames, the spaghet script that runs the fire is swapping out the active mesh over time[1]. I quickly confirmed that the multiple meshes existed but that the batching code didn't think any of them was current due to an incorrect index.

It's a weird one to explain, but in short, the index should have been an int, but instead, it was a byte.

The index was into our global collection of meshes[2], and this meant that if the index was <256, the mesh would work. If not, it wouldn't.

It worked when loading a board that already has the tesla-coil as (purely by chance) it was one of the first 255 meshes added to the global collection.

It didn't work when importing the mod's board and THEN adding the tesla-coil because that board had a lot of stuff loaded already, and so the index of the tesla-coil's mesh ended up being >255

This also explains why I couldn't reduce the board easily. It wasn't the number of assets in the board that mattered, but the variety. When I tried to trim the board to make it smaller, I was removing just enough kinds of assets that the tesla mesh would end up with a valid index again.

Looking at our git history, I think I was going to use a local mesh index at first (and so chose byte), but partway through, I switched to indexing the global pool, evidently without updating the type from byte to int.

So! Now to ship it. I've checked out the TaleSpire code from before all the HeroForge work, and I have cherrypicked the fix commits over. I hope to ship these fixes tomorrow. It's tempting to rush it out tonight, but I'd rather have an extra pair of eyes on it first.

#### Build-Id

Until now, we tried to include the Steam build-id in the UI to help with bug reports. However, the steam command to get the build-id is super weird:

> int GetAppBuildId();
>
> Gets the buildid of this app, may change at any time based on backend updates to the game.
>
> Returns: int
> The current Build Id of this App. Defaults to 0 if you're not running a build downloaded from Steam.

Did you spot it? It returns the latest build-id on Steam, not the build-id of what you have installed!

Why? I don't know. But I finally got annoyed enough to fix the problem.

I've modified our build scripts to use git to get the short hash of the current TaleSpire commit and include it as a `const string` in the c# code.

It's dead simple and makes it trivial to ensure we are testing the build the players were playing.

For you lovely folks out there who are dll hacking and making unofficial mods, we append an extra `_M` to the end of the build-id when the `AppStateManager.UsingCodeInjection` flag is set.

This flag has been hugely helpful for helping sort bugs reports on the server side, so thank you so much for setting it. By including it in the build-id, we can speed up triaging some of the bug reports we've seen so far that have been mod-related.

#### In house tooling

Today I picked up a task that has been on my todo list for ages. Improve in-editor tools for working with tiles and props.

Tiles and props in TaleSpire are not made with Unity's GameObjects. We made enormous performance improvements by creating a custom approach [3], but in doing so, we have lost the fantastic tooling we got trivially with GameObjects.

To improve this situation, I wrote a custom inspector that uses our pixel-picking code to display a bunch of our internal asset data. It's ugly, but it's already proving useful.

As I was apparently in the mood to work on stuff that had been waiting forever, I decided to optimize the physics debug drawing code. Until now, we used Unity's [Handles](https://docs.unity3d.com/ScriptReference/Handles.html) API, and it is slow. Slow to the point that I can't visualize physics info for large boards.

Luckily the `Debug.DrawLine` method was updated to work from [Burst](https://docs.unity3d.com/Packages/com.unity.burst@1.7/manual/index.html) compiled jobs, so I decided to rewrite `Handles` to use `Debug.DrawLine` instead.

This only took an hour or two, and the result was a HUGE boost in performance. However, there is a cap on the number of lines that can be drawn per frame, so my next step will be to replace `Debug.DrawLine` with my own approach. However, for now, this is great.

With that done, I tweaked the new inspector to enable visualizing the physics data for the selected asset. This used to be pretty tedious to debug, and now it's two clicks :)

#### HeroForge

The AssetDb work was required for HeroForge as well as modding, so we've covered the bulk of the news. However, the HeroForge team has really gone to town packing extra data into their models so that we can hopefully support 'Dark Magic' as well as a bunch of other things.

As just one example, they now include metadata about their emissive textures, which helps us process them much faster in some cases. This is all stuff they didn't have to do, so we're super grateful!

#### And that's the lot

It's been a busy time, and it is not slowing down. The next asset pack is well underway, the next patch is ready for testing, and I'll start integrating all the new stuff from HeroForge tomorrow.

Until next time. Stay safe.


*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] A missing texture causing odd behavior on some GPUs

[1] We also use standard vertex animations.

[2] This is an approximation, but it will do.

[3] For a reminder of how slow it was, even with lots of caching and work, check out the video in this dev log: https://bouncyrock.com/news/articles/talespire-dev-log-202
