Twitter Analysis
================

This lets you analyse Tweets over an elapsed period. It has an input flow that runs continuously, and a web client that shows the analysis.

Web Client
----------

![ScreenShot](/Screenshot.bmp)

The Web Client shows a word-cloud of all the words that were tweeted about the chosen subject over the last 24 hours. The Colour of the words is determined by the sentiment
of the tweets, and the size of word is determined by the number of followers of the Tweets.

If you click a word, it will show the tweets that contain that word.


The Web Client uses AJAX/JSON to access the JSON database directly (The web client itself is a static HTML/CSS/JS file). The database is accessed via a proxy `http://vdd.bluemix.net/db/` to avoid cross-origin browser issues.

Here is the flow for the Web Client and the db Proxies

![Web Client Flow](/WebClientFlow.bmp)


Twitter Input Flow
------------------


This flow runs continuously and registers with Twitter for tweets with a certain search query (the screenshot is for 'Vodafone').
It performs sentiment analysis on the tweet to give a score of bad (-100) to (+100) for each Tweet. It then stores the Tweet as a JSON document in the Cloudant Database. This is the structure for an example tweet:

```
{"_id":"tweet:539965460179984385",
"_rev":"1-820957d9921b341ef8f8b8071500d602",
"topic":"tweets/cric_champ",
"payload":["You","and","I..in","this","","beautiful","","world","","?","best","Vodafone","","ad","??"],
"location":"Incredible India",
"lang":"en",
"tweet":
	{"created_at":"Wed Dec 03 02:12:52 +0000 2014","id":539965460179984400,
	"id_str":"539965460179984385",
	"text":"You and I..in this  beautiful  world  ? best Vodafone  ad ??",
	"source":"<a href=\"http://twitter.com/download/android\" rel=\"nofollow\">Twitter for Android</a>",
	"truncated":false,"in_reply_to_status_id":null,
	"in_reply_to_status_id_str":null,
	"in_reply_to_user_id":null,
	"in_reply_to_user_id_str":null,
	"in_reply_to_screen_name":null,
	"user":
		{"id":304800091,"id_str":"304800091",
		"name":"12th  Man ","screen_name":"cric_champ",
		"location":"Incredible India","url":"http://favstar.fm/users/cric_champ",
		"description":"63 not  out","protected":false,
		"verified":false,"followers_count":11709,
		"friends_count":1632,"listed_count":77,
		"favourites_count":25422,"statuses_count":134055,
		"created_at":"Wed May 25 03:56:21 +0000 2011",
		"utc_offset":19800,"time_zone":"Chennai",
		"geo_enabled":false,"lang":"en",
		"contributors_enabled":false,"is_translator":false,
		"profile_background_color":"FD0A60",
		"profile_background_image_url":"http://pbs.twimg.com/profile_background_images/686777381/0a3b1d2eafa8f28b37787a47b34027d9.jpeg",
		"profile_background_image_url_https":"https://pbs.twimg.com/profile_background_images/686777381/0a3b1d2eafa8f28b37787a47b34027d9.jpeg",
		"profile_background_tile":false,
		"profile_link_color":"111EDD",
		"profile_sidebar_border_color":"DEF2D4",
		"profile_sidebar_fill_color":"E3DEB1",
		"profile_text_color":"F1AC5B",
		"profile_use_background_image":true,
		"profile_image_url":"http://pbs.twimg.com/profile_images/539625093551226881/WTLk6oBn_normal.jpeg",
		"profile_image_url_https":"https://pbs.twimg.com/profile_images/539625093551226881/WTLk6oBn_normal.jpeg",
		"profile_banner_url":"https://pbs.twimg.com/profile_banners/304800091/1417491673","default_profile":false,
		"default_profile_image":false,"following":null,"follow_request_sent":null,"notifications":null},
	"geo":null,"coordinates":null,"place":null,"contributors":null,"retweet_count":0,
	"favorite_count":0,"entities":{"hashtags":[],"trends":[],"urls":[],"user_mentions":[],"symbols":[]},"favorited":false,"retweeted":false,
	"possibly_sensitive":false,"filter_level":"medium","lang":"en","timestamp_ms":"1417572772437"},
"sentiment":
	{"score":6,"comparative":0.4,
	"tokens":["you","and","iin","this","","beautiful","","world","","","best","vodafone","","ad",""],"words":["beautiful","best"],
	"positive":["beautiful","best"],"negative":[]}
}
```

For each word in the Tweet, the flow checks that the word is a real-word (using Wictionary.com) and then stores the Word in the database, linking back to the Tweet. If the word
already exists, it updates the existing word. Here is an example word JSON document:

```
{"_id":"word:best","_rev":"735-041c99c7d7b0d1721bbd7a2577dc4b96","created_at":"Tue Dec 23 10:43:42 +0000 2014",
"word":"best",
"tweet_id":
	[{"id":"539965460179984385"},{"id":"539972774434201602"},{"id":"540001761730527233"},{"id":"540563965538238464"}],
"sentiment":3.488143870470413,
"followers_count":23}
```

Here is the flow for the TwitterInput:

![Twitter Input Flow](/TwitterInputFlow.bmp)


There is also a flow that removes tweets after a configurable time (currently set to 24 hours). The flow removes the tweet and any links to the tweet from words it contains. If it removes the last link for a word, it also deletes the word from the database.

![Twitter Purge Flow](/TwitterPurgeFlow.bmp)

