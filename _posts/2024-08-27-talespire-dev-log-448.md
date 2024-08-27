---
layout: post
title: TaleSpire Dev Log 448 - When you accidentally a localization system
description:
date: 2024-08-27 22:37:54
category:
tags: ['Bouncyrock', 'TaleSpire']
---

<iframe width="800" height="600" src="https://www.youtube.com/embed/AbSehcT19u0?si=KIj9MLXqqREedYs6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

This is a video of me recently.

So I'm meant to be working on dice. I was. And I will. But I was chatting to Chairmander, and we agreed that it was a bit of a shame that we've done so much work on the tagging, but you folks haven't seen any of it yet. It feels like just[0] having the new search implementation would make things so much nicer.

So, I took a day and worked on the new search. And I hated the code I wrote. 

I had another go. Hmm, this is tricky. What am I missing?

I realized I was making it harder for myself as the tag type was very work in progress. I needed to nail down what the tag data layout should be.

After a bit of poking around, I started making progress, but one crucial detail of tags was still unexplored. Translation.

Now, translation is its own kettle of fish. We currently use [Unity.Localization](https://docs.unity3d.com/Packages/com.unity.localization@1.5/manual/index.html), which has some nice aspects, but we want to allow the community to be able to translate not just the game, but also each other's creations. For that, we need something a bit more dynamic. Also, it's dependent on [Addressables](https://docs.unity3d.com/Manual/com.unity.addressables.html), which has caused us no end of issues with our builds; we've been longing to get rid of it for ages.m/Manual/com.unity.addressables.html), which has caused us no end of issues with our builds. We've been longing to get rid of it for ages.

I did a bunch of research into localization last year, so I dug out those resources and spent an evening going through them again. With my brain loaded back up, I dove in. Before I knew it, I had evaluated a couple of libraries, decided on what we needed, and started work on replacing `Unity.Localization`.

I replaced every text UI element in TaleSpire with our subclass that hooked into our new text and localization registry. I migrated all the existing translations to the new system[1], and then it was time to add a language dropdown to the settings window.

Now, our settings system was an experiment from years back, and it was a bit of a failure[2]. It works okay for you folks, but it's a bit of a mess behind the scenes. It will not work for the language setting as we want to be able to use the setting inside the Unity editor, even when the game isn't running. This leaves me two options: I can either make a settings file just for the localization, or I can bite the bullet and rewrite the whole damn settings system.

So I'm rewriting the whole damn settings system :P

This is going well. I should get most of the settings bit done tomorrow.

That'll let me get the localization working.

Which will let me implement the tags system with a better understanding of how language will be represented.

Which will let me implement search. 

And that will let me get back to dice!

This is all a little silly, and I could, of course, switch back any time. But this progress is hugely valuable, so I will keep riding this wave and see how much we can get out of it.

I hope you are all well.

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] "Just", the most devilish word in the english language.
[1] We only had a very incomplete Norwegian translation, but it's good for testing. 
[2] This is fine; lots of experiments fail. But we've been living with this one for a while.
