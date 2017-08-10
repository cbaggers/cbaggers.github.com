---
layout: post
title: Scrabbling back onto the wagon
description:
date: 2017-08-10 09:07:31
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

I fucked up for a few weeks by not doing these. Sorry about that.

Update will be easy as I have been in 'absorb phase' for a few weeks and so I've not been coding much in my free time.

Aside from watching a few films and playing a few games my intake has mainly been around the 'data oriented design' space. I've been rewatching these:

[code::dive conference 2014 - Scott Meyers: Cpu Caches and Why You Care](https://youtu.be/WDIkqP4JbkE)
[CppCon 2014: Chandler Carruth - Efficiency with Algorithms, Performance with Data Structures](https://youtu.be/fHNmRkzxHWs)
[CppCon 2014: Mike Acton - Data-Oriented Design and C++](https://youtu.be/rX0ItVEVjHc)

And then

[code::dive 2016 conference – Chandler Carruth – Making C++ easier, faster and safer (part 1)](https://youtu.be/cX_GhJ6BuWI)
[code::dive 2016 conference – Chandler Carruth –Making C++ easier, faster, safer (part 2)](https://youtu.be/E36BUcTWo50)
[On "simple" Optimizations - Chandler Carruth - Secret Lightning Talks - Meeting C++ 2016](https://youtu.be/s4wnuiCwTGU)
[2013 Keynote: Chandler Carruth: Optimizing the Emergent Structures of C++](https://youtu.be/eR34r7HOU14)
[Things that Matter - Scott Meyers | DConf2017](https://youtu.be/RT46MpK39rQ)

I've also been reading the wonderful [What every programmer should know about memory](http://futuretech.blinkenlights.nl/misc/cpumemory.pdf) which is poor in name but outstanding in content. It's really for programmers who need to get everything out of the machine and folks who need to understand why they aren't. I'm not finished yet but its already been very helpful.

From that my interest in assembly programming was growing again. The primary reason is that lisp has a disassemble function that lets you see what your function got compiled to. Here is a destructive 'multiply a vector3 by a float' function:

	CL-USER> (disassemble #'v3-n:*s)
	; disassembly for RTG-MATH.VECTOR3.NON-CONSING:*S
	; Size: 51 bytes. Origin: #x22966965
	; 65:       F30F105201       MOVSS XMM2, [RDX+1]              ; no-arg-parsing entry point
	; 6A:       F30F59D1         MULSS XMM2, XMM1
	; 6E:       F30F115201       MOVSS [RDX+1], XMM2
	; 73:       F30F105205       MOVSS XMM2, [RDX+5]
	; 78:       F30F59D1         MULSS XMM2, XMM1
	; 7C:       F30F115205       MOVSS [RDX+5], XMM2
	; 81:       F30F105209       MOVSS XMM2, [RDX+9]
	; 86:       F30F59CA         MULSS XMM1, XMM2
	; 8A:       F30F114A09       MOVSS [RDX+9], XMM1
	; 8F:       488BE5           MOV RSP, RBP
	; 92:       F8               CLC
	; 93:       5D               POP RBP
	; 94:       C3               RET
	; 95:       0F0B10           BREAK 16                         ; Invalid argument count trap

Cool, but naturally not much good if I can't read it. So that is my driving factor: Understanding this ↑↑↑↑.

I have been repeatedly advised to start with something simpler that x64 asm, but i just cant find any motivation, when it comes down to it I dont want to program for the c64 any more, and arm/mips holds no appeal until I have a need for it. So against better judgement I picked up the [AMD64 Architecture Programmer’s Manual Volume 1](https://support.amd.com/TechDocs/24592.pdf) and started reading.. and it's nice. I quickly run into things I dont get of course but then it's off to youtube again where [kupula's series of x86_64 Linux Assembly tutorials](https://www.youtube.com/playlist?list=PLetF-YjXm-sCH6FrTz4AQhfH6INDQvQSn) gave me a nice soft intro. 

This is a clearly the start of a *looooong* road, but I'm in no rush, and everything I learn is directly helping me grok that disassembly above.

I think that's it. I'm still streaming and still struggling with the procedural erosion stuff..turns out there are more bugs in the paper than expected :|

But that's for another day

Seeya!
