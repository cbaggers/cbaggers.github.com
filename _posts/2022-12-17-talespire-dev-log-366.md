Heya folks!

I'm dropping in to let you know what I've been up to these last few days. 

## Hiding board list from players

I've done some work on the option for hiding the board list from players. I wired up the behind-the-scenes stuff and then handed it over to Ree as I was having some trouble with the UI portion.

All went well, so we expect this to be the next feature that ships.

## Mac bug

In [log 364](https://bouncyrock.com/news/articles/talespire-dev-log-364), Ree talked about a bug blocking mac support and raised a ticket with Unity. I decided to spend a couple of days looking at it in case I could make a workaround. Spoiler, I couldn't.

To trigger the bug, you have a light (which renders its volume without writing to depth buffer), and then you have something modify the depth in that area. What is extra odd is that it requires that you forward-render something for it to show up. It's super weird. Check out this daft shit:

![wat](/assets/videos/macGlitch.gif)

Looking at the Metal frame trace in XCode, the most obviously wrong thing is that the GPU actions from later command-buffers seem to be executed in an earlier command encoder. I don't know enough about Metal to work out why this is causing the errors, but it is feasible that it's related.

Unity has confirmed the issue, so we are waiting to see what comes next.

## Persisting initiative

Today I spent my time working on the code to allow us to persist data from board-wide systems. This is going well. It's hard to write much about as it's mainly me poking around to find something I like. I will need to make another pass on this sometime as patterns are emerging between the different managers that persist board data.

Once I've got this done (I'm thinking at least one more day of work), hooking up the initiative manager should be trivial. I'm currently expecting this feature to ship late next week.

## And that's ya lot

Despite the mac bug, it feels good to be making steady progress. I don't foresee any issues with persisting the initiative list. I'll get right on to the Mod.io slab integration as soon as that works!

I hope this finds you all well.

Peace.


*Disclaimer: This DevLog is from the perspective of one developer. It doesn't reflect everything going on with the team*
