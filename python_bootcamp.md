---
layout: default
img: python
img_link: http://xkcd.com/353/
caption: Hello world!
title: "Python Bootcamp"
active_tab: homework
release_date: 2016-01-27
due_date: 2016-01-29T14:00:00EST
---

<!-- Check whether the assignment is up to date -->
{% capture this_year %}{{'now' | date: '%Y'}}{% endcapture %}
{% capture due_year %}{{page.due_date | date: '%Y'}}{% endcapture %}
{% if this_year != due_year %} 
<div class="alert alert-danger">
Warning: this assignment is out of date.  It may still need to be updated for this year's class.  Check with your instructor before you start working on this assignment.
</div>
{% endif %}
<!-- End of check whether the assignment is up to date -->

Python Bootcamp 
=============================================================
(Thanks Ellie Pavlick for the original material, which is adapted to Python 3.x version here)

We will start writing some code! This bootcamp is designed to be a crash-course to get you up to speed on Python programming. 

### 0. Installation of Python and/or Jupyter Notebook

For this bootcamp, I will recommend following [this guide](http://jupyter.readthedocs.io/en/latest/install.html) to install Anaconda, that conveniently includes Python, the Jupyter Notebook and other commonly used packages for scientific computing and data science.

Alternatively, if you have trouble installing Anaconda, you can try installing [standalone Python](https://www.python.org/downloads/). 

### 1. The basics: variables and data structures

Python has the basic variable types you are used to: strings, ints, floats. Unlike Java and many other languages, variables are not type-checked. You simply declare a variable by assigning a value to it. Later, you can reassign a different type to that same variable and Python couldn't care less.

Open up the python interpreter and play with variable assignment and reassignment:


{% highlight tcsh %} 
$ python
{% endhighlight %}

{% highlight python %} 
# You can comment with pound sign
"""
Or with triple quotes
"""
>>> x = 2
>>> x
2
>>> y = "hello world"
>>> y
'hello world'
>>> x = y # Notice the lack of whining about "incompatible types"...
>>> x
'hello world'
{% endhighlight %} 

This also means that you can mix variable types within a data structure. There is no need to specify that L is a list of ints or that M is a map from strings to floats.

Lists are declared with square brackets and indexed using square bracket notation. They can also be treated as stacks, if you are into that sort of thing. 

Create a list of ints. Then, in order to drive those Scala people insane, start appending strings to it. Play with indexing and slicing. In Python, you can use the colon notation to pull out slices of a list. E.g. lst[i:j] will give you a new list which includes the ith through the (j-1)th elements of lst.

{% highlight python %}
>>> l = [1, 2, 3]
>>> for elem in l: #We'll talk about loops more in a bit
...	print elem 
... 
1
2
3
>>> l.append("i am a string. mwahahaha.") 
>>> for elem in l: 
...	print elem 
... 
1
2
3
i am a string. mwahahaha.
>>> l[2]
3
>>> l[1:3]
[2, 3]
>>> l += ['here is more stuff', 6, [2,3,4], 5*1367] 
>>> print l
[1, 2, 3, 'i am a string. mwahahaha.', 'here is more stuff', 6, [2, 3, 4], 6835]
>>> l.pop() 
6835
{% endhighlight %}

Dictionaries (or maps or associative arrays) are probably the favorite data structure of Python. They are a simple key/value store, again without any restrictions on which data types are the keys or values. You can declare dictionaries with curly braces and associate or retrieve keys and values using square bracket notation.


{% highlight python %}
>>> d = {"give me an A!" : "B", "give me a P!" : 7, "give me a Q!" : "no."}
>>> d["give me a P!"]
7
>>> d[14] = 12
>>> d
{'give me a Q!': 'no.', 'give me an A!': 'B', 14: 12, 'give me a P!': 7} # the new k/v pair was added to d
{% endhighlight %}

### 2. Control structures and functions

Python makes it easy to write bad code. But it makes it _very_ hard to write ugly code. So chalk one up for superficiality. Python uses whitespace to denote control structures, like loops and if/else blocks. By convention, you should use four spaces for each level of indentation. 

{% highlight python %}
>>> print l
[1, 2, 3, 'here is more stuff', 6, [2, 3, 4]]
# Here is a for loop. 
>>> for elem in l:
...     print elem
... 
1
2
3
here is more stuff
6
[2, 3, 4]
# Here is a while loop
>>> i = 0
>>> while i < 5 : 
...     if i % 2 == 0 : 
...             print "even"
...     else : 
...             print "odd"
...     i += 1
... 
even
odd
even
odd
even
"""
No types are required for parameters, so commenting is SO important. So important.
Returns the idx element of a list

lst - the list 
idx - the integer index of the element to return
"""
>>> def get_list_element(lst, idx) : 
...     return lst[idx]
... 
>>> get_list_element(l, 4)
6
{% endhighlight %}

### 3. File IO

You can open, read, and write files using the aptly-named open(), read(), and write() commands. read() returns the entire contents of the file as a string. readlines() will split on the newline character and return the lines as a list, which is generally nicer for allowing you to iterate line-by-line. I won't go through an example here, but I highly recommend playing with the [csv module](https://docs.python.org/2/library/csv.html), which is incredibly useful and we will likely use regularly throughout the semester. 


{% highlight python %}
>>> file = open('test.txt', 'w')
>>> for s in ['line1', 'line2', 'line3', 'line4'] : 
...     file.write(s+'\n')
... 
>>> file.close()
>>> contents = open('test.txt').read()
>>> contents
'line1\nline2\nline3\nline4\n'
>>> contents = open('test.txt').readlines()
>>> contents
['line1\n', 'line2\n', 'line3\n', 'line4\n']
{% endhighlight %}

### 4. Text processing in Python

For this part, you will need to write your code to answer the following questions. You should download the [Jupyter notebook](assignments/python-bootcamp/IPythonBootcamp.ipynb) file, and do all of your work there. 
 
We will be playing with a small but oh so wonderful data set of wine reviews! You can download the data [here](assignments/python-bootcamp/data.tgz). You can unpack it as follows, and should see two files:


{% highlight tcsh %}
$ tar -xzvf data.tgz 
x data/
x data/stopwords.txt
x data/wine.txt
$ ls data
stopwords.txt	wine.txt
{% endhighlight %}

wine.txt is in the format of one review per line, followed but a star rating between 1 and 5 (except for 3 reviews, where the review decided to go rogue and give 6 stars. Pft.) The text of the review and the star rating are separated by a single tab character. There is also a file called stopwords.txt. You will use this in question 6.

Write a python script that answers each of the following questions and prints the answer to standard output:

1. What is the distribution over star ratings?
2. What are the 10 most common words used across all of the reviews, and how many times is each used?
3. How many times does the word 'a' appear?
4. How many times does the word 'fruit' appear?
5. How many times does the word 'mineral' appear?
6. Common words (like 'a') are not as interesting as uncommon words (like 'mineral'). In natural language processing, we call these common words "stop words" and often remove them before we process text. stopwords.txt gives you a list of some very common words. Remove these stopwords from your reviews. Also, try converting all the words to lower case (since we probably don't want to count 'fruit' and 'Fruit' as two different words). Now what are the 10 most common words across all of the reviews, and how many times is each used?
7. You should continue to use the preprocessed reviews for the following questions (lower-cased, no stopwords).  What are the 10 most used words among the 5 star reviews, and how many times is each used? 
8. What are the 10 most used words among the 1 star reviews, and how many times is each used? 
9. Gather two sets of reviews: 1) Those that use the word "red" and 2) those that use the word "white". What are the 10 most frequent words in the "red" reviews which do NOT appear in the "white" reviews?
10. What are the 10 most frequent words in the "white" reviews which do NOT appear in the "red" reviews?

You can compare your answers against [our key](assignments/python-bootcamp/bootcamp-key.txt) to see if you have done things correctly. I highly recommend looking into the functions available in the [python string module](https://docs.python.org/2/library/string.html).

That's it! 

### Bonus! Bash bootcamp.

Knowing more than one scripting language increases your productivity 1 zillion fold (proven fact). If you breezed through the python bootcamp and are sitting and twiddling your thumbs, try brushing up your bash programming skills by doing the following questions using the same wine.txt file. Many of them are the same or similar to what you just did in python. Think about how these operations are conceptually different when you write in bash compared to python. Check out this [cheat sheet](http://crowdsourcing-class.org/bash-commands.html) of bash commands to get you started.

1. How many lines are there in the file?
2. What is the distribution over star ratings?
3. How many reviews contain the word 'a'?
4. How many reviews contain the word 'fruit'?
5. How many reviews contain the word 'mineral'?
6. Make a new file containing the full text of all the reviews, with one word per line. (You don't have to do this in python, but I think that is the easiest way. If you want to try a new command-line tool, check out [sed](http://stackoverflow.com/questions/1853009/replace-all-whitespace-with-a-line-break-paragraph-mark-to-make-a-word-list)). 
7. How many total words appear in your list?
8. How many unique words appear in your list?
9. What are the 10 most common words used across all of the reviews, and how many times is each used?
10. How many times does the word "red" appear? (Be careful of capitalization!)
