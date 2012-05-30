---
layout: post
title: Gnome blogger sans paragraphs
description: 
category:
tags: ['python']
---

For those who want to use gnome blog poster sans paragraphs the fix is to modify /usr/lib/pymodules/python2.7/gnomeblog/rich_entry.py

<pre class="brush: python">
Replacing line 23
	self.set_pixels_below_lines(16)
with
	self.set_pixels_below_lines(0)

..and..

Replacing line 26 &amp; 27
	html_converter.para_tag.opening_tag = "&lt; p&gt;"
        html_converter.para_tag.closing_tag = "&lt; /p&gt;"
with
        html_converter.para_tag.opening_tag = "&lt; br&gt;"
        html_converter.para_tag.closing_tag = ""&lt; br&gt;</pre>
I'll probably post a patch some time as there is a bug reported in bugzilla about this.
</pre>