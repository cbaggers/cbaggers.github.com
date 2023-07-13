Heya folks!

Today I come with a screenshot.

![mac build without errors](assets/images/macNoErrors.png)

That screenshot shows the macOS build working without errors.
This is great news in that we are again one more step toward shipping this blighter.

The tradeoff is that, for the first release, we (bouncyrock) will need to build the assets separately for Windows and Mac. That is very normal, but it's not something we want for modding. We want you to be able to build your mods once and have them work on any platform we support.

To that end, I think we will need to make a custom asset format before we ship creature, tile, or prop mods. That's fine though. I was expecting that to be the case. Another upside of this is that we untie our assets from the Unity version, so upgrades will be less risky for us in the future[0].

Once we have our own format, we can use that in place of Unity's asset-bundles and remove these separate Windows/macOS asset builds.

Alrighty, time to go get these changes merged in. Hopefully, next week we can get back to looking at Steam's process for shipping games on macOS.

Until then,

Peace.

*Disclaimer: This DevLog is from the perspective of one developer. So it doesn't reflect everything going on with the team*

[0] The worry is that a new version might require that assets be rebuilt to use some feature, and once the community makes mods, that becomes nearly impossible to ensure. This makes us less likely to take that risk, which in turn means TaleSpire can miss out on fixes and feature improvements in later versions of Unity.
