---
layout: default
title: Twitter API Tutorial
img: failwhale
caption: Twitter's 404 error page -- the Fail Whale
img_link: http://business.time.com/2013/11/06/how-twitter-slayed-the-fail-whale/
active_tab: tutorial
---

Twitter API tutorial 
=============================================================

**by [Wei Xu](https://cocoxu.github.io/)** <a class="twitter-follow-button" data-show-count="false" href="https://twitter.com/cocoweixu">Follow @cocoweixu</a> <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script> **&nbsp;&nbsp;(Ohio State University)**

**updated by [Jeniya Tabssum](https://sites.google.com/site/jeniyatabassum/)** <a class="twitter-follow-button" data-show-count="false" href="https://twitter.com/JeniyaTabassum">Follow @JeniyaTabassum</a> <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script> **&nbsp;&nbsp;(Ohio State University)**


Last updated Dec 2, 2018 (added a script for obtaining all followers of a Twitter user; updated with tweepy package); originally written July 1, 2015. 


[[download the Jupyter notebook for this tutorial](twittertutorial.ipynb.json){:target="_blank"}]


### 1. Getting Twitter API keys

To start with, you will need to have a Twitter developer account and obtain credentials (i.e. API key, API secret, Access token and Access token secret) on the to access the Twitter API, following these steps:

- Create a Twitter developer account if you do not already have one from : [https://developer.twitter.com/](https://developer.twitter.com/){:target="_blank"}
- Go to [https://developer.twitter.com/en/apps](https://developer.twitter.com/en/apps){:target="_blank"} and log in with your Twitter user account.
- Click "Create an app"
- Fill out the form,  and click "Create"
- A pop up window will appear for reviewing Developer Terms. Click the "Create" button again.
- In the next page, click on "Keys and Access Tokens" tab, and copy your "API key" and "API secret" from the `Consumer API keys` section. 
- Scroll down to `Access token & access token secret` section and click "Create". Then copy your "Access token" and "Access token secret".


### 2. Installing  Twitter library

We will be using a Python library called [Tweepy](https://github.com/tweepy/tweepy){:target="_blank"} to connect to Twitter API and downloading the data from Twitter. There are [many other libraries](https://developer.twitter.com/en/docs/developer-utilities/twitter-libraries){:target="_blank"} in various programming languages that let you use Twitter API. We choose the Tweepy for this tutorial, because it is simple to use yet fully supports the Twitter API. 

- Install tweepy by using pip/easy_install to pull it from PyPI:

{% highlight csh %}
$ pip install tweepy
{% endhighlight %}


You may also use Git to clone the repository from GitHub and install it manually:

{% highlight csh %}
$ git clone https://github.com/tweepy/tweepy.git
$ cd tweepy
$ python setup.py install
{% endhighlight %}


### 3. Connecting to Twitter Streaming APIs

The Streaming APIs give access to (usually a sample of) all tweets as they published on Twitter. On average, about 6,000 tweets per second are posted on Twitter and you (normal dev users) will get a small proportion (<=1%) of it. The Streaming APIs are one of the two types of Twitter APIs. The other one called REST APIs (we will talk about later in this tutorial), which is more suitable for singular searches, such as searching historic tweets, reading user profile information, or posting Tweets. The Streaming API **only** sends out real-time tweets, while the Search API (one of the popular REST APIs) gives historical tweets up to about a week with a max of a couple of hundreds. You may request elevated access (e.g. Firehose, Retweet, Link, Birddog or Shadow) for more data by contacting Twitter's API support. 

#### Basic Uses of Streaming APIs

Create a file called *twitter_streaming.py*, and copy the code below into it. Make sure to enter your credentials obtained in the Step 1 above into ACCESS_TOKEN, ACCESS_SECRET, CONSUMER_KEY, and CONSUMER_SECRET.


{% highlight python %}
# Import the necessary package to process data in JSON format
try:
    import json
except ImportError:
    import simplejson as json

# Import the tweepy library
import tweepy

# Variables that contains the user credentials to access Twitter API 
ACCESS_TOKEN = 'YOUR ACCESS TOKEN"'
ACCESS_SECRET = 'YOUR ACCESS TOKEN SECRET'
CONSUMER_KEY = 'YOUR API KEY'
CONSUMER_SECRET = 'ENTER YOUR API SECRET'

# Setup tweepy to authenticate with Twitter credentials:

auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_SECRET)

# Create the api to connect to twitter with your creadentials
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True, compression=True)
#---------------------------------------------------------------------------------------------------------------------
# wait_on_rate_limit= True;  will make the api to automatically wait for rate limits to replenish
# wait_on_rate_limit_notify= Ture;  will make the api  to print a notification when Tweepyis waiting for rate limits to replenish
#---------------------------------------------------------------------------------------------------------------------


#---------------------------------------------------------------------------------------------------------------------
# The following loop will print most recent statuses, including retweets, posted by the authenticating user and that user’s friends. 
# This is the equivalent of /timeline/home on the Web.
#---------------------------------------------------------------------------------------------------------------------

for status in tweepy.Cursor(api.home_timeline).items(200):
	print(status._json)
	
#---------------------------------------------------------------------------------------------------------------------
# Twitter API development use pagination for Iterating through timelines, user lists, direct messages, etc. 
# To help make pagination easier and Tweepy has the Cursor object.
#---------------------------------------------------------------------------------------------------------------------



{% endhighlight %}



If you run the program by typing in the command:

{% highlight csh %}
$ python twitter_streaming.py
{% endhighlight %}

You will see tweets from your homepage in your screen. They are most recent statuses, including retweets, posted by the you and that your friends. The data returned is in [JSON](https://en.wikipedia.org/wiki/JSON){:target="_blank"} format. It may looks too much for now; it will become clearer in the next step how to read and process this data. Below is one example tweet: 

<p align="center"><img src="https://raw.githubusercontent.com/cocoxu/socialmedia-class.github.io/master/assets/img/my_tweet.png" align="center" style="border:1px solid grey" height="200" /></p>




and its JSON format (often output without line breaks to save space but difficult to read and make sense of --- but you can do pretty printing to include line breaks to make it more readable as shown in Step 5) :

{% highlight javascript %}
{"favorited": false, "contributors": null, "truncated": false, "text": "#CFP Workshop on Noisy User-generated Text at ACL - Beijing 31 July 2015. Papers due: 11 May 2015. http://t.co/rcygyEowqH   #NLProc #WNUT15", "possibly_sensitive": false, "in_reply_to_status_id": null, "user": {"follow_request_sent": null, "profile_use_background_image": true, "default_profile_image": false, "id": 237918251, "verified": false, "profile_image_url_https": "https://pbs.twimg.com/profile_images/527088456967544832/DnclpoZO_normal.jpeg", "profile_sidebar_fill_color": "DDEEF6", "profile_text_color": "333333", "followers_count": 226, "profile_sidebar_border_color": "C0DEED", "id_str": "237918251", "profile_background_color": "C0DEED", "listed_count": 13, "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png", "utc_offset": null, "statuses_count": 120, "description": "I am a postdoctoral researcher @PennCIS, studying Natural Language Processing and Social Media.", "friends_count": 166, "location": "Philadelphia PA", "profile_link_color": "0084B4", "profile_image_url": "http://pbs.twimg.com/profile_images/527088456967544832/DnclpoZO_normal.jpeg", "following": null, "geo_enabled": true, "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png", "name": "Wei Xu", "lang": "en", "profile_background_tile": false, "favourites_count": 88, "screen_name": "cocoweixu", "notifications": null, "url": "http://www.cis.upenn.edu/~xwe/", "created_at": "Thu Jan 13 23:15:12 +0000 2011", "contributors_enabled": false, "time_zone": null, "protected": false, "default_profile": true, "is_translator": false}, "filter_level": "low", "geo": null, "id": 616333141884674048, "favorite_count": 0, "lang": "en", "entities": {"user_mentions": [], "symbols": [], "trends": [], "hashtags": [{"indices": [0, 4], "text": "CFP"}, {"indices": [124, 131], "text": "NLProc"}, {"indices": [132, 139], "text": "WNUT15"}], "urls": [{"url": "http://t.co/rcygyEowqH", "indices": [99, 121], "expanded_url": "http://noisy-text.github.io", "display_url": "noisy-text.github.io"}]}, "in_reply_to_user_id_str": null, "retweeted": false, "coordinates": null, "timestamp_ms": "1435780246598", "source": "<a href=\"http://twitter.com\" rel=\"nofollow\">Twitter Web Client</a>", "in_reply_to_status_id_str": null, "in_reply_to_screen_name": null, "id_str": "616333141884674048", "place": null, "retweet_count": 0, "created_at": "Wed Jul 01 19:50:46 +0000 2015", "in_reply_to_user_id": null}
{% endhighlight %}


You can run the program and save the data into a file for analysis later using the following commend:
{% highlight csh %}
$ python twitter_streaming.py > twitter_stream_200tweets.txt
{% endhighlight %}

#### Advanced Uses of Streaming APIs
The streaming API provides more advanced functions. 

First, you can set different parameters (see [here](https://developer.twitter.com/en/docs/tutorials/consuming-streaming-data){:target="_blank"} for a complete list) to define what data to request. For example, you can track certain tweets by specifying keywords or location or language etc. The following code will get the tweets in English that include the term "google":



{% highlight python %}

class StreamListener(tweepy.StreamListener):

    def on_status(self, status):
        print(status.text)
        
    def on_error(self, status_code):
        if status_code == 420:
            return False

stream_listener = StreamListener()
stream = tweepy.Stream(auth=api.auth, listener=stream_listener)
stream.filter(track=["google"],languages=["en"])

{% endhighlight %}


**Location is a bit tricky**. Read [here](https://developer.twitter.com/en/docs/tweets/filter-realtime/guides/basic-stream-parameters.html){:target="_blank"} for a simple guide, and [here](http://thoughtfaucet.com/search-twitter-by-location/){:target="_blank"} for a complete guide. Find tweets by location can be done either by the Streaming API (only geolocated tweets) or the Search API.

Second, by default, streaming API is connecting to the "public streams" --- all public data on Twitter as we showed in the above example. Also, we can use the streaming api to get tweets by a specific user. The follow parameter inside the filter fucntion can take an array of IDs to stream. 

{% highlight python %}


stream.filter(follow=["2211149702"])

{% endhighlight %}



### 4. Reading and Processing Tweets in JSON format

The streaming API returns [tweets](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object){:target="_blank"}, as well as several [other types of messages](https://developer.twitter.com/en/docs/tutorials/consuming-streaming-data){:target="_blank"} (e.g. a tweet deletion notice, user update profile notice, etc), all in JSON format.  Here we demonstrate how to read and process tweets in details. Other data in JSON format can be processed similarly. 

Tweets, also known more generically as “status updates”. This map made by Raffi Krikorian  explains a tweet in JSON format:


<img src="https://raw.githubusercontent.com/cocoxu/socialmedia-class.github.io/master/assets/img/raffi-krikorian-map-of-a-tweet.png" width="600" />

Although this map is from 2010 and a bit out-of-date, it is a good visualization of tweet's JSON format. You can find the up-to-date information of tweet's format [here](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object){:target="_blank"}.


Use Python library *json* or *simplejson* to read in the data in JSON format and process them:
{% highlight python %}


# Import the necessary package to process data in JSON format
try:
    import json
except ImportError:
    import simplejson as json

# We use the file saved from last step as example
tweets_filename = 'twitter_stream_200tweets.txt'
tweets_file = open(tweets_filename, "r")


for line in tweets_file:
    try:
        # Read in one line of the file, convert it into a json object 
        tweet = json.loads(line.strip())
        if 'text' in tweet: # only messages contains 'text' field is a tweet
            print(tweet['id']) # This is the tweet's id
            print(tweet['created_at']) # when the tweet posted
            print(tweet['text']) # content of the tweet
                        
            print(tweet['user']['id']) # id of the user who posted the tweet
            print(tweet['user']['name']) # name of the user, e.g. "Wei Xu"
            print(tweet['user']['screen_name']) # name of the user account, e.g. "cocoweixu"

            hashtags = []
            for hashtag in tweet['entities']['hashtags']:
                hashtags.append(hashtag['text'])
            print(hashtags)

    except:
        # read in a line is not in JSON format (sometimes error occured)
        continue
{% endhighlight %}

If, instead of the file *twitter_stream_200tweets.txt*, use the example tweet in JSON format in the Streaming API section, this piece of code will output the following results:

{% highlight text %}
616333141884674048
Wed Jul 01 19:50:46 +0000 2015
#CFP Workshop on Noisy User-generated Text at ACL - Beijing 31 July 2015. Papers due: 11 May 2015. http://t.co/rcygyEowqH   #NLProc #WNUT15
237918251
Wei Xu
cocoweixu
[u'CFP', u'NLProc', u'WNUT15']
{% endhighlight %}

Note that the same url will have a few different versions in the Twitter stream: *http://t.co/rcygyEowqH* in the text, *http://noisy-text.github.io* as the expanded full version, *noisy-text.github.io* as the display version. 

For long-term data collection, you can setup a [cron job](https://en.wikipedia.org/wiki/Cron){:target="_blank"}. If you are interested in running a long term collection of one or multiple streaming queries, consider using [Mark Dredze](http://www.cs.jhu.edu/~mdredze/){:target="_blank"}'s Twitter streaming [library](https://github.com/mdredze/twitter_stream_downloader){:target="_blank"}. This library wraps the basic streaming API with several helpful features, such as organizing data into files by data, support for multiple feed types, and ensuring feeds remain active after interruptions. You can run this library inside of crontab or supervisord.


### 5. Using Twitter Search API, Trends API, and User API

Besides the streaming APIs, Twitter also provide another type of APIs --- REST APIs. It provides two main functionalities: *GET* data from Twitter and *POST* data (e.g. a tweet from your account) to Twitter. In this tutorial, we will demonstrate three most useful APIs to collect data for social media research: Search (tweets contain certain words), Trends (trending topics) and User (a user's tweets, followers, friends, etc.). For explanations of these key types of data offered by Twitter, see the [lecture slides](syllabus.html) on this course website.

#### Search API

Similar to the Streaming API, you first import necessary Python packages and OAuth credentials as in Step 3 and create the api. Then you can use search API like follows:

{% highlight python %}

# Search for latest tweets about "#nlproc"
tweets = tweepy.Cursor(api.search, q='#nlproc')

{% endhighlight %}


Alternatively, you can search with more parameters (see a full list [here](https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets.html)){:target="_blank"}. For example, search for 10 latest tweets about "#nlproc":

{% highlight python %}

# Search for 10 most recent tweets about "#nlproc"
tweets = tweepy.Cursor(api.search, q='#nlproc', count=10)

{% endhighlight %}


Alternatively, you can search with more parameters (see a full list [here](https://developer.twitter.com/en/docs/api-reference-index){:target="_blank"}. For example, search for 10 latest tweets about "#nlproc" in English:


{% highlight python %}

# Search for latest tweets written in english about "#nlproc"
tweets = tweepy.Cursor(api.search, q='#nlproc', lang='en')

{% endhighlight %}



#### Trends API

Twitter provides global trends and as well as localized tweets. The easiest and best way to see what trends are available and the place ids (which you will need to know to query localized trends or tweets), is by using this commend to request worldwide trends:


{% highlight python %}
# Get all the locations where Twitter provides trends service
world_trends = api.trends_available()

{% endhighlight %}




It returns all the trends that are offered by Twitter at the time. Here is part of the returned results:

{% highlight text %}
{"name": "United States", "countryCode": "US", "url": "http://where.yahooapis.com/v1/place/23424977", "country": "United States", "parentid": 1, "placeType": {"code": 12, "name": "Country"}, "woeid": 23424977}

{"name": "San Francisco", "countryCode": "US", "url": "http://where.yahooapis.com/v1/place/2487956", "country": "United States", "parentid": 23424977, "placeType": {"code": 7, "name": "Town"}, "woeid": 2487956}

{"name": "Bangkok", "countryCode": "TH", "url": "http://where.yahooapis.com/v1/place/1225448", "country": "Thailand", "parentid": 23424960, "placeType": {"code": 7, "name": "Town"}, "woeid": 1225448}
{% endhighlight %}


The places ids are WOEIDs (Where On Earth ID), which are 32-bit identifiers provided by [Yahoo! GeoPlanet](https://developer.yahoo.com/geo/geoplanet/guide/concepts.html){:target="_blank"} project. And yes! Twitter is very international. 

After you know the ids for the places you are interested in, you can get the local trends like this:
{% highlight python %}
# Get trending topics in San Francisco (its WOEID is 2487956)
sfo_trends = api.trends_place(id =2487956)
{% endhighlight %}


The trends will be returned in JSON, again. This time we reformat JSON data in a prettier way to make it easier to read by human beings:

{% highlight python %}
print(json.dumps(sfo_trends, indent=4))
{% endhighlight %}

and get:

{% highlight javascript %}
{
   "created_at":"2015-07-01T22:09:55Z",
   "trends":[
      {
         "url":"http://twitter.com/search?q=%23LiesIveToldMyParents",
         "query":"%23LiesIveToldMyParents",
         "name":"#LiesIveToldMyParents",
         "promoted_content":null
      },
      {
         "url":"http://twitter.com/search?q=%22Kevin+Love%22",
         "query":"%22Kevin+Love%22",
         "name":"Kevin Love",
         "promoted_content":null
      },
      
      ... [and another 8 trends omitted here to save space]

}
{% endhighlight %}

If you want to get the real tweets in each trend, use the Search API to get them.

**How often do the Twitter trending topics change?** It is not disclosed by Twitter, but based on my experience, you will get most of them by querying every 5 minutes. 

#### User API

Another popular use of API is to obtain the social graph of users' followers and friends, as well as a particular user's tweets. Below we show two common usage examples of the User APIs:

{% highlight python %}
# Get the full list of followers of a particular user
list_of_followers=[]

current_cursor = tweepy.Cursor(api.followers_ids, screen_name="cocoweixu", count=5000)
current_followers = current_cursor.iterator.next()
list_of_followers.extend(current_followers)
next_cursor_id = current_cursor.iterator.next_cursor

while(next_cursor_id!=0):
	current_cursor = tweepy.Cursor(self.api.followers_ids, screen_name="cocoweixu", count=5000,cursor=next_cursor_id)
	current_followers=current_cursor.iterator.next()
	list_of_followers.extend(current_followers)
	next_cursor_id = current_cursor.iterator.next_cursor
{% endhighlight %}

{% highlight python %}

# Get a particular user's timeline (up to 200 of his/her most recent tweets)
status_cursor = tweepy.Cursor(api.user_timeline, screen_name="billybob", count=200,tweet_mode='extended')
status_list = status_cursor.iterator.next()


{% endhighlight %}





#### Rate Limit 

Twitter’s streaming API allows a limited number of attempts to connect. If clients exceed the maximum allowed attempts in a window of time, they will receive error 420. The amount of time a client has to wait after receiving error 420 will increase exponentially each time they make a failed attempt.

The **REST APIs have a strict rate limit** on how many requests you can send given a time limit and how many tweets you can get access to for each request (and it got stricter and stricter in the past). The limits vary from one API function to another. Twitter's dev website give a list of the rate limits [here](https://developer.twitter.com/en/docs/basics/rate-limiting.html){:target="_blank"}.


You can also query the API to check your remaining quota, though you may rarely use this command:
{% highlight python %}
api.rate_limit_status()
{% endhighlight %}



### 6. Learning More about Twitter APIs

This tutorial is meant to help you to start. To learn more about Twitter APIs, here are two ways I found quite sufficient:

- Look up the [documentation](https://developer.twitter.com/en/docs){:target="_blank"} of Twitter APIs to find the function you would like to use, then search in the [source code](https://github.com/tweepy/tweepy){:target="_blank"} of the Twitter Python Tools to see how to call it in your program.

- Check more example Python scripts that demonstrate interactions with the Twitter API
via the Tweepy Library: 
    [https://github.com/tweepy/examples](https://github.com/tweepy/examples){:target="_blank"}



Please contact us if you have any suggestions or notice any information that becomes out-of-date.   



