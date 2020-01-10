---
layout: post
title: TaleSpire Dev Log 144
description:
date: 2020-01-10 23:13:49
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Today I found and fixed a few nasty bugs, two of which were caught by the automated test generation mentioned yesterday.

I started off by writing the code to handle the undo/redo history. When that looked like it was working I set the automated test generator to make sequences of board changes between 80 and 100 operations long, this very quickly resulted in a series of operations that caused an exception.

The trouble was that it took 90 operations to trigger the bug which likely would be a huge pain to step through, so I made the worlds dumbest test minimizer.

When I get an error in an automated test I have it dump out the sequence of ops as code I can paste as a new test. This is what it gave me

```
[Test]
public void ManualMix3()
{
    using (var asm = new BoardModelTestAssemblage())
    {
        var actions = new List<VTuple<OpName, int, int>>() {
            VTuple.New(OpName.Add, 13, 0),
            VTuple.New(OpName.Delete, 6, 17),
            VTuple.New(OpName.Add, 56, 1),
            VTuple.New(OpName.Add, 23, 4),
            VTuple.New(OpName.Add, 27, 13),
            VTuple.New(OpName.Delete, 3, 17),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 39, 13),
            VTuple.New(OpName.Delete, 36, 7),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 57, 10),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 34, 15),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 11, 17),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 38, 6),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 0, 19),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Add, 33, 2),
            VTuple.New(OpName.Delete, 45, 17),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Add, 5, 1),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 22, 6),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Delete, 38, 3),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 47, 3),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 3, 1),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 14, 12),
            VTuple.New(OpName.Delete, 50, 18),
            VTuple.New(OpName.Add, 13, 16),
            VTuple.New(OpName.Delete, 45, 9),
            VTuple.New(OpName.Add, 42, 12),
            VTuple.New(OpName.Delete, 2, 8),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Delete, 34, 10),
            VTuple.New(OpName.Add, 50, 18),
            VTuple.New(OpName.Add, 48, 16),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Add, 13, 1),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Delete, 7, 12),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 11, 6),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 52, 7),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 31, 16),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 29, 1),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 55, 14),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 4, 18),
            VTuple.New(OpName.Redo, 0, 0),
            VTuple.New(OpName.Add, 26, 2),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Add, 47, 17),
            VTuple.New(OpName.Delete, 25, 6),
            VTuple.New(OpName.Delete, 40, 12),
            VTuple.New(OpName.Add, 16, 4),
            VTuple.New(OpName.Delete, 32, 19),
            VTuple.New(OpName.Add, 0, 13),
            VTuple.New(OpName.Add, 52, 5),
            VTuple.New(OpName.Undo, 0, 0),
            VTuple.New(OpName.Delete, 55, 18),
            VTuple.New(OpName.Delete, 1, 19),
            VTuple.New(OpName.Add, 45, 11),
            VTuple.New(OpName.Redo, 0, 0),
        };
        asm.ApplyActions(actions);
    }
}
```

I wrote a little function that just, at random removed one operation from `actions` and ran the test again. If it still triggered the error it kept the new, shorter, list and repeated the process. If it didn't trigger the error it tried a different operation. It was horrendously slow as it was doing it in dumbest, most brute force way imaginable but..It spat out this:

```
using (var asm = new BoardModelTestAssemblage())
{
	var actions = new List<VTuple<OpName, int, int>>() {
		VTuple.New(OpName.Add, 39, 13),
		VTuple.New(OpName.Delete, 36, 7),
		VTuple.New(OpName.Undo, 0, 0),
		VTuple.New(OpName.Delete, 34, 15),
		VTuple.New(OpName.Undo, 0, 0),
	};
	asm.ApplyActions(actions);
}
```

Much better :D

I then simplified the values to make my job even easier and was able to find the bug itself within 15 minutes. It wasn't one thats exciting to read about, just some simple iteration mistake when undoing deletes of tiles, BUT it was super cool that we can do this.

If you find this fun look up things like quickcheck to see how the pros do it :p

With that and some other bugs fixed I am back to data wrangling.

So in all, today went well. I wish I had got further but that's just how it goes.

Seeya tomorrow
