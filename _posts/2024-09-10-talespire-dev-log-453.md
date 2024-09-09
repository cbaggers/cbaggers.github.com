---
layout: post
title: TaleSpire Dev Log 453
description:
date: 2024-09-09 21:28:26
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good evening all,

On a bit of a roll with dev-logs today. Today, I've been working on the translation file format. 

The goal is for a translator to go into TaleSpire, choose a language, and click export to produce a file that has all the strings that need translation for that language.

As usual with human stuff, it's never that simple.

> If you like a more complete and entertaining version of this, check out this Tom Scott video. https://youtu.be/0j74jcxSunY

Let's say you want to display: "{creature-name} has {some-number of} apples."

In the case of one apple, we'd probably want to write "{creature-name} has 1 apple", not "1 apples" so plurality is something we will need to care about.

Let's say the number of apples is in parameter `@1`. We could have some simple rules where the value is used to pick the correct text. For example:

`{creature-name} has {@1=1:an apple|@1 apples}`.

This syntax could mean that if `@1` equals 1, use `an apple`. If not, use `@1 apples`.

Seems like a good start, but many languages have more complicated systems[0]. Luckily, the Unicode Consortium has assembled a [big ol' table of pluralizing rules](https://www.unicode.org/cldr/charts/45/supplemental/language_plural_rules.html). Just look at that monster! This does help, though, as translation libraries can implement these rules to provide a tool for their translators.

Most language complications are not this readily solved, though. Translation tools generally just punt and let the translator handle the issues. Which, honestly, is the right move. You want a human translation, and many texts need significant restructuring to make them feel native.

The grammatical gender of words is another thing that will come up a lot. If you, like me, were raised in a language which didn't have grammatial genders for nouns it can feel very confusing to hear that a city is female or a door is male or whatever. This is because we conflate it with sexual gender (at least, I certainly did), which it's not. It's quite simply the category of the noun[1] (often called a noun class IIRC). It frequently has some overlap with sexual gender, though, which can make things extra confusing[2]. So, let's talk about these categories.

> Just gotta shout out Worrorra, which also has "terrestrial," "celestial," and "collective" genders. How dope is that?!

Right, so in our "apples" example above, languages will translate the text differently based on the class of both "apples" and the creature. The word "apples" is hard-coded into the string so it can be handled there, but the creature-name is passed in from the game. What we can do is support passing values (like gender) to the translation library and let the translator pattern match those parameters. Of course, in all cases, the translator also has to provide a version of the text for the "unspecified" case. In the future, we'll need to give you various options so that, if you want (and if it makes sense), you can specify the gender of your characters.

So that's a little ramble about some complications with translations. At the end of the day, you just gotta give the translators decent information and let them work their magic. 

I've written a small specification for our file format. It'll allow for pattern matching on the input arguments and a few kinds of inline formatting tools within the translation string. Those formatters include:

- Simple list handling
- A plural form selector (following the [rules from unicode](https://cldr.unicode.org/index/cldr-spec/plural-rules))
- Picking the nth of some number of forms based on a parameter or conditional checks

I don't need to implement all this tomorrow, though. My current goal is to get our (very wip) Norwegian translation working again and get this system merged into master. That translation doesn't use any fancy features yet, so I can implement the rest another day.

Well, that's all from me for now. 

Seeya!

*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*

[0] And some languages, like Indonesian, have no differentiation! Which is awesome. This is the equivalent of "one sheep, many sheep" in English.

[1] Gender, in this case, derives from "genus," meaning "kind," which more clearly highlights that relation to "category."

[2] In French, you get cases where you can end up referring to a rabbit using the masculine class, even if you know the rabbit is female. 
