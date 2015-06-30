---
layout: default
img: failwhale
caption: Twitter's 404 error page -- the Fail Whale
img_link: http://business.time.com/2013/11/06/how-twitter-slayed-the-fail-whale/
title: Twitter API Tutorial
active_tab: tutorial
---

Twitter API tutorial 
=============================================================

**by [Wei Xu](http://www.cis.upenn.edu/~xwe/) (July 2015)** &nbsp;&nbsp;&nbsp;&nbsp; <a class="twitter-follow-button" data-show-count="false" href="https://twitter.com/cocoweixu">Follow @cocoweixu</a> <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>

###1. Getting Twitter API keys

To start with, you will need to have a Twitter account and obtain credentials (i.e. API key, API secret, Access token and Access token secret) on the Twitter developer site to access the Twitter API, following the following steps:

- Create a twitter account if you do not already have one.
- Go to [https://apps.twitter.com/](https://apps.twitter.com/) and log in with your twitter user account.
- Click "Create New App"
- Fill out the form, agree to the terms, and click "Create your Twitter application"
- In the next page, click on "Keys and Access Tokens" tab, and copy your "API key" and "API secret". Scroll down and click "Create my access token", and copy your "Access token" and "Access token secret".

###2. Installing a Twitter library

We will be using a Python library called [Python Twitter Tools](https://pypi.python.org/pypi/twitter) to connect to Twitter API and downloading the data from Twitter. There are many other [libraries](https://dev.twitter.com/overview/api/twitter-libraries>) in various programming languages that let you use Twitter API. We choose the Python Twitter Tools for this tutorial, because it is simple yet fully supports the Twitter API. 

Download the Python Twitter tools at [https://pypi.python.org/pypi/twitter](https://pypi.python.org/pypi/twitter).

Install the Python Twitter Tools package by typing in commands:


{% highlight csh %}
$ python setup.py --help
$ python setup.py build     
$ python setup.py install
{% endhighlight %}


###3. Connecting to Twitter Streaming APIs
The Streaming APIs give access to (usually a sample of) all tweets as they published on Twitter. On average, about 6,000 tweets are posted on Twitter and you (normal users) will get a small proportion of it (<=1%). The Streaming APIs are one of the two types of Twitter APIs. The other one called REST APIs (we will talk about later in this tutorial), which is more suitable for singular searchers, such as searching historic tweets, reading user profile information, or posting Tweets.

Create a file called twitter_streaming.py, and copy the code below into it. Make sure to enter your credentials obtained in the Step 1 above into ACCESS_TOKEN, ACCESS_SECRET, CONSUMER_KEY, and CONSUMER_SECRET.

{% highlight python %}
# Import the necessary methods from "twitter" library
from twitter import Twitter, OAuth, TwitterHTTPError, TwitterStream

# Variables that contains the user credentials to access Twitter API 

ACCESS_TOKEN = 'YOUR ACCESS TOKEN"'
ACCESS_SECRET = 'YOUR ACCESS TOKEN SECRET'
CONSUMER_KEY = 'YOUR API KEY'
CONSUMER_SECRET = 'ENTER YOUR API SECRET'

# Initiate the connection to Twitter Streaming API
twitter_stream = TwitterStream(auth=OAuth(ACCESS_TOKEN, ACCESS_SECRET,
            CONSUMER_KEY, CONSUMER_SECRET))

# Get a sample of the public data following through Twitter
iterator = twitter_stream.statuses.sample()

# Print each tweet in the stream to the screen 
# Here we set it to stop after getting 1000 tweets. 
# You don't have to set it to stop, but can continue running the Twitter API for days or even longer. 
tweet_count = 1000
for tweet in iterator:
    tweet_count -= 1
    print tweet     
    if tweet_count <= 0:
        break     
{% endhighlight %}

If you run the program by typing in the command:

{% highlight csh %}
$ python twitter_streaming.py
{% endhighlight %}

You will see tweets keep flowing in your screen like below. They are a sample of public tweets flowing though Twitter at the moment. You can stop this program by pressing Ctrl-C after a little while.


The tweets you got are in [JSON](https://en.wikipedia.org/wiki/JSON) format (we will explain how to read and process this data format in the next step). Here shows one normal tweet and two deleted tweets: 
{% highlight javascript %}
{u'favorited': False, u'contributors': None, u'truncated': False, u'text': u'Hurry up Friday \U0001f64c http://t.co/agty1tdHlg', u'possibly_sensitive': False, u'in_reply_to_status_id': None, u'user': {u'follow_request_sent': None, u'profile_use_background_image': True, u'default_profile_image': False, u'id': 170432073, u'verified': False, u'profile_image_url_https': u'https://pbs.twimg.com/profile_images/612554207615414272/CBGW8Nc9_normal.jpg', u'profile_sidebar_fill_color': u'F6F6F6', u'profile_text_color': u'333333', u'followers_count': 357, u'profile_sidebar_border_color': u'EEEEEE', u'id_str': u'170432073', u'profile_background_color': u'ACDED6', u'listed_count': 0, u'profile_background_image_url_https': u'https://abs.twimg.com/images/themes/theme18/bg.gif', u'utc_offset': None, u'statuses_count': 3485, u'description': u'Life is like a cup of tea.. Its all in how you make it. \u2615\ufe0f', u'friends_count': 537, u'location': u'England', u'profile_link_color': u'038543', u'profile_image_url': u'http://pbs.twimg.com/profile_images/612554207615414272/CBGW8Nc9_normal.jpg', u'following': None, u'geo_enabled': True, u'profile_banner_url': u'https://pbs.twimg.com/profile_banners/170432073/1428346836', u'profile_background_image_url': u'http://abs.twimg.com/images/themes/theme18/bg.gif', u'name': u'Ella MacDonald', u'lang': u'en', u'profile_background_tile': False, u'favourites_count': 1549, u'screen_name': u'Ella_MacDonald', u'notifications': None, u'url': None, u'created_at': u'Sat Jul 24 20:34:22 +0000 2010', u'contributors_enabled': False, u'time_zone': None, u'protected': False, u'default_profile': False, u'is_translator': False}, u'filter_level': u'low', u'geo': None, u'id': 616000231851823104, u'favorite_count': 0, u'lang': u'en', u'entities': {u'symbols': [], u'media': [{u'expanded_url': u'http://twitter.com/Ella_MacDonald/status/616000231851823104/photo/1', u'display_url': u'pic.twitter.com/agty1tdHlg', u'url': u'http://t.co/agty1tdHlg', u'media_url_https': u'https://pbs.twimg.com/media/CIx47n-W8AAQ-Em.jpg', u'id_str': u'616000214781063168', u'sizes': {u'large': {u'h': 643, u'resize': u'fit', u'w': 640}, u'small': {u'h': 341, u'resize': u'fit', u'w': 340}, u'medium': {u'h': 602, u'resize': u'fit', u'w': 600}, u'thumb': {u'h': 150, u'resize': u'crop', u'w': 150}}, u'indices': [18, 40], u'type': u'photo', u'id': 616000214781063168, u'media_url': u'http://pbs.twimg.com/media/CIx47n-W8AAQ-Em.jpg'}], u'hashtags': [], u'user_mentions': [], u'trends': [], u'urls': []}, u'in_reply_to_user_id_str': None, u'retweeted': False, u'coordinates': None, u'timestamp_ms': u'1435700874661', u'source': u'<a href="http://twitter.com/download/iphone" rel="nofollow">Twitter for iPhone</a>', u'in_reply_to_status_id_str': None, u'in_reply_to_screen_name': None, u'id_str': u'616000231851823104', u'extended_entities': {u'media': [{u'expanded_url': u'http://twitter.com/Ella_MacDonald/status/616000231851823104/photo/1', u'display_url': u'pic.twitter.com/agty1tdHlg', u'url': u'http://t.co/agty1tdHlg', u'media_url_https': u'https://pbs.twimg.com/media/CIx47n-W8AAQ-Em.jpg', u'id_str': u'616000214781063168', u'sizes': {u'large': {u'h': 643, u'resize': u'fit', u'w': 640}, u'small': {u'h': 341, u'resize': u'fit', u'w': 340}, u'medium': {u'h': 602, u'resize': u'fit', u'w': 600}, u'thumb': {u'h': 150, u'resize': u'crop', u'w': 150}}, u'indices': [18, 40], u'type': u'photo', u'id': 616000214781063168, u'media_url': u'http://pbs.twimg.com/media/CIx47n-W8AAQ-Em.jpg'}]}, u'place': None, u'retweet_count': 0, u'created_at': u'Tue Jun 30 21:47:54 +0000 2015', u'in_reply_to_user_id': None}
{u'delete': {u'status': {u'user_id_str': u'1229226626', u'user_id': 1229226626, u'id': 604938438706499584, u'id_str': u'604938438706499584'}, u'timestamp_ms': u'1435700874976'}}
{u'delete': {u'status': {u'user_id_str': u'724203342', u'user_id': 724203342, u'id': 263466740904767488, u'id_str': u'263466740904767488'}, u'timestamp_ms': u'1435700874993'}}
{% endhighlight %}

You can run the program and save the data into a file for analysis later using the following commend:
{% highlight csh %}
$ python twitter_streaming.py > twitter_stream_100tweets.txt
{% endhighlight %}

The streaming API provides more advanced functions -- you can find more information [here](https://dev.twitter.com/streaming/overview/request-parameters). For example, you can track certain tweets by specifying keywords or location or language etc. The following code will get the tweets in English that include the term "Google". 
{% highlight javascript %}
iterator = twitter_stream.statuses.filter(track="Google", language="en")
{% endhighlight %}

###4. Reading and Processing Tweets in JSON format

To Do -- last edit on June 30, 2015

###5. Twitter Search API


[https://dev.twitter.com/rest/public/search](https://dev.twitter.com/rest/public/search)

{% highlight python %}
# Initiate the connection to Twitter API
t = Twitter(auth=OAuth(ACCESS_TOKEN, ACCESS_SECRET,
            CONSUMER_KEY, CONSUMER_SECRET))
            
# Search for ten latest tweets about "#nlproc"
t.search.tweets(q='#nlproc', count=10)
{% endhighlight %}


Alternatively, you can search with more parameters
# For example, search for one latest tweets about "#nlproc" in English
{% highlight python %}
t.search.tweets(q='#nlproc', result_type='recent', lang='en', count=1)
{% endhighlight %}

###6. Checking out More Examples 

Check more example Python scripts that demonstrate interactions with the Twitter API
via the Python Twitter Tools module: 
   
   [https://github.com/ideoforms/python-twitter-examples](https://github.com/ideoforms/python-twitter-examples)