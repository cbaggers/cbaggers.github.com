---
layout: post
title: TaleSpire Dev Log 422
description:
date: 2023-11-13 23:33:56
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Heya folks,

Today was a tedious one.

I finished up last week by moving a whole bunch of code out of our Unity project and into a separate C# project that referenced the Unity DLLs. We are doing this as we want to ship TaleWeaver as compiled code[0].

I finished that task, and the TaleSpire project compiled, but when I ran it, it crashed immediately. The reason is that by moving all the code to a separate project, all of the references from prefabs and scenes to that code broke.

To understand why, we have to know that Unity wants developers to be able to rename and move things without breaking all those references. So, rather than referring to the classes by name[1], they create a GUID for everything in the project. As they can't push those GUIDs into the files they are identifying, they instead make a separate file called a `meta` file, which stores the ID and any other metadata for that thing[2].

As meta files are a Unity concept, I can't include them in our new C# library. So, by moving the code, I broke all the references!

Of course, Unity does need a way to identify types inside a C# library, and I found this unofficial info talking about how it works: [https://www.robinryf.com/blog/2017/10/30/unity-behaviour-in-dlls.html](https://www.robinryf.com/blog/2017/10/30/unity-behaviour-in-dlls.html)

I now have the tools I need to fix this issue.

- First, I will check out a build from before I moved the code out to the separate library.
- Then, I will write a script to collect the GUID for every script in the project[3]. I will make a lookup table between the GUIDs and the fully-qualified type names.
- I can then check out the most recent code (the one with all the broken references), scan the project for any use broken references, and use the GUID to find the fully qualified type name.
- With the type and the sample code from the link above, I can calculate the new reference information and replace the broken reference.

If I'm correct, I can automate the entire repair, which is a relief as I have done fixes for issues like this before, and they SUCKED.

In the future, I might even be able to use this knowledge to make tools to help fix issues that occur during merges sometimes.

Well, wish me luck for tomorrow. We'll see how this all turns out.

Peace.

p.s. The MD4 hashing code from the linked blog post was helpful but performed a bunch of unnecessary heap allocations. I ended up procrastinating earlier today by tweaking it to be more efficient. You can find that here: [https://gist.github.com/baggers-br/48bba51a9e0b1c5a0632c57537f84534](https://gist.github.com/baggers-br/48bba51a9e0b1c5a0632c57537f84534)
It is almost entirely untested, so please beware.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] I know, I know. Byte-code. But still, you know what I mean.
[1] Fully-qualified or otherwise
[2] If you are very curious, you can find some info about this here: [https://unityatscale.com/unity-meta-file-guide](https://unityatscale.com/unity-meta-file-guide)
[3] In fact I'm only including subclasses of MonoBehaviour
