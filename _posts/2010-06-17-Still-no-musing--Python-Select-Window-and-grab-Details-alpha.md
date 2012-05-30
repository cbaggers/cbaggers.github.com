---
layout: post
title: Still no musing - Python Select Window and grab Details (alpha)
description: 
category:
tags: ['python']
---

I have had need of a program that gave me the ability to select any window on the desktop and have it return details for me.
Turns out there is a lovely command line program specifically for this called 'xprop'. To use it in python I have called subprocess and got it to rip the results that contain the information I need into a dictionary.

Its very hacky at the mo and will receive plenty of love before it is ready but as I spent so long trying to find out there was a program for this I pretty much had to post it for prosperity.

You have been warned!

<pre class="brush: python;"> 
import subprocess
import tempfile

class WindowGrabber:
    def __init__(self):
        pass
    
    def window_picker(self):
        out = tempfile.TemporaryFile()
        in_pipe = None
        self.job = subprocess.Popen(["xprop",],
                                    stdin=in_pipe,
                                    stdout=out,
                                    stderr=subprocess.STDOUT)
        while self.job.returncode is None:
            self.job.poll()
        out.seek(0)
        result = out.readlines()
        window_details = {}
        for line in result:
            if line.find(" = ") &gt;= 0:
                key, value = line.split(" = ")
                if value.find(", ") &gt;= 0:
                    value = value.split(", ")
                window_details[key] = value
        print ""
        
if __name__ == "__main__":
    a=WindowGrabber().window_picker()
</pre>