---
layout: post
title: TaleSpire Dev Log 115
description:
date: 2019-07-01 21:40:22
category:
tags: ['BouncyRock', 'TaleSpire']
---

Let's get back into the swing of these!

During the Kickstarter I'm using any free time I have to study and make little systems to get familiar with systems we may end up using

Today I've mainly been getting familiar with [blittable types](https://docs.microsoft.com/en-us/dotnet/framework/interop/blittable-and-non-blittable-types). The reason for this is that Unity's [DOTS](https://unity.com/dots) primarily operates on them.

I got interested in how easy it would be to de/serialize such data, given these types can have precise memory layouts specified for them. The that end I was making little `BinaryWriter`s and readers that had Write methods that could take any blittable type or array of blittables and handle them correctly.

I'm going to make a dump of some useful links as most of the stuff I was doing was simple enough, but was a good exercise for familiarity. It was the first time I've significant use of pointers in c# and it was a very simple and unsurprising experience.

The first handy thing was that the [unmanaged constraint](https://devblogs.microsoft.com/premier-developer/dissecting-new-generics-constraints-in-c-7-3/) is available. Whilst unmanaged types are not the same as blittable types there is a useful overlap. Using this allows us to have signatures like `unsafe void Foo<T>(T value) where T : unmanaged`. Also note that `sizeof` is defined to work on unmanaged types. It was very easy to write generic methods doing useful stuff with c#, I'm please with it so far.

- [Scoped GC pin](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/fixed-statement)
- [Heap allocate](http://msdn.microsoft.com/en-us/library/s69bkh17.aspx), check out all of Marshal, it's pretty handy
- [more info on blittables](https://aakinshin.net/posts/blittable/)
- [create a struct from data referenced by pointer](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.marshal.ptrtostructure?view=netframework-4.8#System_Runtime_InteropServices_Marshal_PtrToStructure_System_IntPtr_System_Object_)
- You can explicitly cast typed pointers to `IntPtr`
- [Unsafe](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.unsafe?view=dotnet-plat-ext-2.1) seems like a handy class until you see that it's [not implemented in mono](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Runtime.CompilerServices/Unsafe.cs)
- But then you find [UnsafeUtility](https://docs.unity3d.com/ScriptReference/Unity.Collections.LowLevel.Unsafe.UnsafeUtility.html) in Unity itself and it's way better. `memcpy`, `malloc` etc are all there

The big disappointment of the day is how quickly one ends up having to reinvent the wheel as most Streams, Buffers, Writers, etc, never expose their buffer or provide overloads taking `IntPtr`. The more you poke to work around this the more it feels like you should just make your own. It's not ideal. Saying that, I have spent most of today in dotnet code rather than Unity's so I do still need to see what is there.

Until tomorrow,
Ciao.
