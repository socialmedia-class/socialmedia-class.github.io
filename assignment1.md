---
layout: default
img: languagemix
caption: Twitter's Language Mix in 2013  
img_link: http://www.technologyreview.com/graphiti/522376/the-many-tongues-of-twitter/
title: Homework 1 | Twitter's Language Mix
active_tab: homework
---



Twitter's Language Mix <span class="text-muted">: Assignment 1 (15 points)</span> 
=============================================================

<div class="alert alert-info">
  <p>Due Feb 1 (20% penalty per day for late submission)</p>
</div>


This assignment includes using Twitter's streaming API, an off-the-shell language identification tool and data visualization. Some of the questions are open-ended, which means that there is no single best answer (do the best you can) and the grading will not be strict. You are encouraged to think creatively and go above-and-beyond in this class.

##### 1. Get >=10k tweets from Twitter Streaming API following the instructions on [Twitter API tutorial](/twittertutorial.html) 

##### 2. and check:
- are all tweets LangID tagged (what %) by Twitter?
- how many different language tags provided by Twitter?
- what % is each language?

##### 3. then install/run [langid.py](https://github.com/saffsd/langid.py) and check:
- how many different language tagged?
- what % langid.py and Twitter’s API agree/disagree?
- what kind of tweets/languages do they disagree?

##### 4. what about tweets in US?
- what % is each language?
- what % of tweets are geotagged?

##### 5. draw some fancy plots 
- For example, the language mix in Twitter like the Figure 5 and 7 in [this paper](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0061981)
- Matplotlib is a Python package for plotting, [here](http://matplotlib.org/users/pyplot_tutorial.html) is a simpe guide of Matplotlib. 


Pack your data and code into a zip file named like hw1_yourOSUdotid.zip, that contains a *.py file which is your code that can outputs the answers in the order of questions with some brief text to be self-explainable, and a few image files for question 5 or possibly code to generate the plots. This is not a strict requirement --- think what is the clearest way to present your homework that the TA or instructor can understand and check easily. Submit your homework in [OSU's Carmen](https://carmen.osu.edu/) system.






