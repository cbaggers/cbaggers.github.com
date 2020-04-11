---
layout: post
title: TaleSpire Dev Log 168
description:
date: 2020-04-11 22:05:44
category:
tags: ['TaleSpire', 'Bouncyrock']
---

Heya folks,

Well it's the first dev log after the beta release and in a surprise to no-one except me I'm exhausted today :D
Had a good sleep but clearly my body knows the release is done and fancies a bit more.

Today I did start looking at board sync issues though as anything that breaks boards is clearly a high priority. 

To aid in the process as we hunt this down I've added a small icon to request a board upload. It's temporary as we want all persistence of boards to be totally transparent to players, however clearly it isnt right now. It also doubles as an indicator that shows you when the game knows there are changes that need to be persisted. It looks like this

After the usual 5 second delay it will automatically upload and then icon will change back to it's default state.

I also found a very ugly bug that could cause boards to be corrupted if it was synronising due to switching boards. What would happen is that on completion of an upload the metadata for the current board (including its id) would be updated. However uploads are async so if you created a new board and if the upload took a while (multiple seconds) then it could have concluded when you were already in the new board. This would then overwrite the current board info with info from the last board. This is daft beahviour that is a hangover from the alpha.

I doubt it accounts for many of the issues seen but it's certainly one I'm glad we'll be rid of soon.

I've implemented the change to the game itself and a related change to the backend and tested on my local setup. I'm not going to put out a patch tonight as it's 22:00 here and I have a glass of port that needs a drink and 'breath of the wild' that needs playing (also it's very much tempting fate to push last thing at night :p)

I'll probably push this patch tomorrow.

The release was wild, I should probably write an update on just that soon. There were veyr last second (as in 15 minute before release) server changes which have resulted in DNS entries not having propegated in time (an obvious oversight on my part). Given the usual 48-72 hour propegation times we are hoping to see a drop in the number of cases of the 'stuck at login screen' by end of Monday. After that other cases will be a different bug and we'll attack that!

I'll also be going through the github issues during the week. For now my focus will be on the most board-breaking issue but in time we'll get through them.

Thank you all for the amazing support, both in reporting issues but also just being so good about these bumps as we ride out way though them.

I hope you are all doing well and have been able to get far enough to see where we want to take this thing. Some of the creations we've seen you make already have been mind-blowing. This year is going to be great!

Have a great night folks, 

Seeya with another update real soon.

