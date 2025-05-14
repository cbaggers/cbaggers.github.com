---
layout: post
title: TaleSpire Dev Log 452
description:
date: 2024-09-09 09:34:26
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Good morning folks,

Work has been steady, if not glamorous. I've rewritten the settings system so it's easier to maintain. I'd hoped it would be a lot simpler, but that didn't turn out to be the case. There are some problems where it feels like you only push the complexity around.

That said, I know I could do a lot better job with a real macro system (à la Common Lisp), and that is making me consider biting the bullet and starting to use [compile-time code generation](https://docs.unity3d.com/Manual/roslyn-analyzers.html).

I'm not doing that now, as we have so much to do, and we don't need me going on a one-month diversion in code generation.

With that done, I got back into localization. I worked on the types for identifiying languages in our system. After a bunch of reading, I've settled on a triplet of:

- A mandatory [ISO 639-2](https://en.wikipedia.org/wiki/ISO_639) code for the language
- An optional [ISO 15924](https://en.wikipedia.org/wiki/ISO_15924) code to indicate the script
- And an also optional [ISO 3166-1 alpha 2](https://en.wikipedia.org/wiki/ISO_3166-1) code for the region.

This lets you have codes like `es-HN` (Spanish - Honduras), `en-GB` (English - Great Britain), and `zh-Hant-TW` (Traditional Chinese as used in Taiwan).

Those in the know will notice a significant overlap with [IETF language tags](https://en.wikipedia.org/wiki/IETF_language_tag), which I originally planned to use. However, that standard includes many things we couldn't really hope to handle, such as calendar info and specifications down to the city level.

Given that this is about trying to encode something very human, it gets messy. For example, not every possible language is in the [ISO 649-2](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes) list. Also, languages change, and countries are born and consumed; it's a fluid world out there.

Even simple things like "let's have a drop-down list of supported languages" can be fraught. Let's see how. In addition to making official translations, we are making it possible for the community to create translations for not only community content but TaleSpire itself. Let's say someone makes a Chinese language translation regionalized for Taiwan (remember that regions often have their own ways of using the 'same' language[0]). They say the translation file is for language `zh-Hant-TW`. Cool. I stick that code into C#'s [CultureInfo](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=net-8.0) type and look at the [DisplayName](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.displayname?view=net-8.0#system-globalization-cultureinfo-displayname) property, and we get "Chinese." Ah.

Well, it's not strictly wrong, but it gives the same display name for traditional Chinese (`zh`), so now we can't tell the two apart in our drop-down; why might this be? Hmm, well, IIRC Microsoft is involved with the IETF language standards body, so let's see what the standard says the name for `TW` is.

> Taiwan, Province of China

Ooookay. Well. That *is* the legal status according to the UN. But I doubt that is what all Taiwanese citizens want to see in the language list[1]. To confuse things even more, if I use [this online c# repl](https://dotnetfiddle.net/), it gives me `Chinese (Traditional, Taiwan)`.

All this is to say, stuff is tricky and almost inherently political. It feels like `Chinese (Traditional, Taiwan)` is simpler while still being correct, and that is what I currently have for that entry in the system. But we'll see how it goes. Of course, these entries themselves will need translating into various languages :)

Oh, and let's remember, there are fictional languages, too. If we have an ID for representing languages, we could also use it to identify languages in fictional worlds. And let's be honest, one of you nutters will probably want to translate TaleSpire into Elvish! While the IETF supports some constructed and fictional languages (e.g. Klingon), it couldn't cover all of them.

To handle this, our codes can either be the ISO triplet described above or a randomly generated unique identifier[2]. The unique ID then identifies a community-specified language. We could have used this for the world languages, too, but one nice thing about the triplets (other than there being official standards for us to use) is that it's easy to pick an okay fallback translation. For example, if there isn't `en-GB`, translation, `en` will do fine.

So things are good. Today, I'm working on the translation file format. Once that is done, I'll port our partial Norwegian translation to it, test it, and then merge all this into the master.

See you in the next devlog!

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*


[0] I'm not sure how major this is for Taiwan. I'm using this example due to the display name issue mentioned after.
[1] Especially as (according to Wikipedia) Taiwan no longer represents itself as a member of the UN since the UN recognized the PRC.
[2] It's similar to a GUID in that the likelihood of ID collision is vanishingly small.
