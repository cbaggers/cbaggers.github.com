---
layout: post
title: Whack my sausage
description: 
category:
tags: ['python']
---

The other day I sauntered in from work to find the girlfriend and her brother hollering bizarre, two word phrases and cackling while hammering them into google.
They were, having just finished watching '<a href="http://www.davegorman.com/projects_googlewhack_adventure.html">Dave Gorman's Googlewhack Adventure</a>', manically in search of a googlewhack of their own.
For the uninitiated a googlewhack is, as laid down by the great Wikipedia
<blockquote>..a kind of a contest for finding a Google search query consisting of exactly two words without quotation marks, that return exactly one hit. A Googlewhack must consist of two actual words found in a dictionary</blockquote>

As each minute passed they each plundered deeper into their lexicons until finally, SCIOLIC NUTCRACKER!
A googlewhack, at the time that is; sadly the nature of these things is that once it has been registered, google sees the post and then there are more pages it knows of with the phrase and the googlewhack is no more.

But imminent death aside the glory remained! Al (the brother) had been putting forward many a suggestion with sausage as the second word and was wondering if there were any googlewhacks to be found with his beloved meat product as the star. Of course within a trice my trusty python editor was open and a program was growing...a program to Google search every adjective in a dictionary + sausage!

Alas once you start hammering Google with a good few thousand searches you realise that they all start failing and that you have broken the terms of usage agreement...Bugger

Ah well it was an interesting exercise and as I'm not going to hone a program I cant use, I may as well post it here! Aren't you grateful, you human receptacle of unwanted programs you?!

Enjoy...and don't run it,it will only bring momentary smiles and quick, swift dissatisfaction.... insert your own trite conclusion on the nature of life here.
Ciao!

<pre class="brush: python;"> 
#this program tries to find googlewhacks with the second word as sausage!
import sys
from xgoogle.search import GoogleSearch, SearchError    
import urllib
import simplejson
from threading import Thread


class whackHunter(Thread):
    def __init__(self, phrase):
        Thread.__init__(self)
        self.phrase = phrase
        self.status = -1
        
    def run(self):
        query = urllib.urlencode({'q' : self.phrase})
        url = 'http://ajax.googleapis.com/ajax/services/search/web?v=1.0&amp;%s'\
            % (query,)
        search_results = urllib.urlopen(url)
        json = simplejson.loads(search_results.read())
        result_base = json['responseData']
        if result_base:
            results = result_base['results']
            self.status = len(results)
        else:
            self.status = -2
    

#load in dictionary
dict_file = open("index.adj")
dictionary = (line.split(" ")[0] for line in dict_file)
keep_searching = True
keep_watching = True
hunt_queue = []

while keep_searching:
    while len(hunt_queue)&lt;=5 and keep_watching:
        try:
            search_phrase = &quot;%s sausage&quot; % (dictionary.next(),)
            new_search = whackHunter(search_phrase)
            hunt_queue.append(new_search)
            new_search.start()
        except:
            &quot;thats the whole barrel!&quot;
            keep_watching=False
        
    for i in range(len(hunt_queue)-1)[::-1]:
        if hunt_queue[i].status!=-1:
            search = hunt_queue.pop(i)
            if search.status == 1:
                print &quot;OH YEAHHHH! the whack is %s&quot; % (search.phrase,)
                keep_searching= False
                break
            elif search.status == -2:
                print &quot;dead search %s&quot; % (search.phrase,)
            else:
                print &quot;no joy with %s&quot; % (search.phrase,)        
        
dict_file.close()
print &quot;Done&quot;
</pre>