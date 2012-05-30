---
layout: post
title: Googlizer for Windows
description: 
category:
tags: ['python']
---

I find having to work on Windows for work can be really grating when some of the simple things I am used to on Ubuntu are missing. One such niggle comes from the lack of a beutifully simple program called 'Googlizer'

Quite simply the Googlizer searches google for the last thing you copied to the clipboard, this works for error reports, documents, basically any text you can copy.

Its too simple a program not to have, so here is my version for Windows
<blockquote>
<div id="_mcePaste">#windows googlizer</div>
<div id="_mcePaste">import webbrowser</div>
<div id="_mcePaste">import win32clipboard</div>
<div>import win32con</div>
<div id="_mcePaste">win32clipboard.OpenClipboard()</div>
<div id="_mcePaste">selected_text = win32clipboard.GetClipboardData(win32con.CF_UNICODETEXT)</div>
<div id="_mcePaste">win32clipboard.CloseClipboard()</div>
<div id="_mcePaste">webbrowser.open("http://www.google.com/search?q=%s" % (selected_text,))</div></blockquote>
This has been tested on XP with <a title="ActivePython" href="http://www.activestate.com/activepython/" target="_blank">ActivePython</a>. The code is obviously free to copy, share, mangle, etc.

Feel free to throw me any suggestions!

[EDIT] Fixed bug that caused null value errors on some copied texts