---
layout: post
title: "Building an RSS Twitter Bot"
date: 2014-02-17 13:07:55 +0100
comments: true
categories: [Ruby, Gems, APIs]
---

Last night after finishing my Ruby Notes on the File class I decided I wanted to try building an RSS Twitter bot.  I’d gotten the idea a week or so ago while reading some articles about my Alma Mater’s Top 10 Basketball team, the SDSU Aztecs in case you’re wondering, and thinking it would be cool to create an app that I could plug RSS feeds into and it’d tweet out the updates to the RSS feeds.  This way I’d always be up to date with sports news about my Aztecs and it’d be a good simple project for me to tackle.
<!-- more -->  

{% img center /images/Twitter_Application_Management.png 'SDSUSports Twitter App' 'An image my Twitter bot app.' %}

<h2>Reading RSS Feeds</h2>
So I looked into how to read RSS feeds and found the RSS module and the Simple RSS gem.  I played around with both and decided to use SimpleRSS.  To keep things simple my goal was to plug in two feeds, the <a href="http://www.utsandiego.com/news/sports/sdsu-aztecs/" target="_blank">UT San Diego’s SDSU Sports section</a> and the <a href="www.youtube.com/user/goaztecscom‎" target="_blank">SDSU Athletics Youtube Channel</a> and tweet out the feeds.

Here is what the Twitter Account looked like after I got this to work.

{% img center /images/Cut_Down_Nets___SDSUsports__on_Twitter.png 'SDSUSports Twitter Account' 'An image of the twitter account I created for my twitter bot to post to.' %}

<h2>Getting Off Track</h2>
It didn’t take long to get the basics of taking in a feed and parsing it to remove just the title and the link but after that I got a bit off track.  I spent a bunch of time on how to remove the date that was included at the end of every GoAztecs.com Youtube video title because I decided I didn’t want this in the tweets.  Here’s an example title so you know what I’m talking about, “SDSU MEN'S HOOPS: STEVE FISHER POSTGAME vs. AIR FORCE - 2/15/14.”  This turned out to be good practice with strings and arrays, but in hindsight this was a dumb thing to do since this is for one specific feed and thus can’t be reused.  What I should have been focusing on was sorting feeds as either an RSS feed or Atom feed, because that dictates what methods you use to parse the feed.

<h2>Sort Them Feeds</h2>
Once I finished removing the dates from the video titles I went about sorting whether the feeds were RSS or Atom.  I knew that having “feed” and “rss” in the URL was probably not the standard for telling if a feed was RSS or Atom but I just wanted to get this working for these two feeds and improve my code later. 

```ruby Sorting Feeds
def feed_sorter
feed_num = 0 

	@feeds.each do |feed|
		feed_num +=1
		case 
		when feed.include?('feed')
			@sorted_feeds[feed_num] = [feed, 'atom']
		when feed.include?('rss')
			@sorted_feeds[feed_num] = [feed, 'rss']
		else invalid feed
		end 
	end 
end 
```
You can see that I’m checking if “RSS” or “Atom” is in the URL and then sticking then storing them in an array inside a hash.  Again I knew I’d have to change this down the road, especially if I start using a database for persistence but this gets the job done for my two feeds.

<h2>Putting it All Together</h2>
From here all I wanted to clean up my code and put it in a class.  So I created a RssParser class, which probably should be renamed to FeedParser, and created the methods initialize, run, feed_sorter, feed_parser, and send_tweet. Then I added the Twitter Gem and my keys and tokens and boom it worked.

```ruby RssParser 
require 'simple-rss'
require 'open-uri'
require 'twitter'

class RssParser 

	def initialize(*feeds)
		@feeds = feeds 
		@sorted_feeds = {}
		@tweet = ''
		
		@client = Twitter::REST::Client.new do |config|
		  config.consumer_key        = "Consumer_Key"
		  config.consumer_secret     = "Consumer_Secret"
		  config.access_token        = "Access_Token"
		  config.access_token_secret = "Secret_Token"
		end
	end 
		

	def run
		feed_sorter
		feed_parser
	end 

#method to sort if feeds are atom or rss feed
	def feed_sorter
		feed_num = 0 

		@feeds.each do |feed|
			feed_num +=1
			case 
			when feed.include?('feed')
				@sorted_feeds[feed_num] = [feed, 'atom']
			when feed.include?('rss')
				@sorted_feeds[feed_num] = [feed, 'rss']
			else invalid feed
			end 
		end 
	end 

#method to parse feed to retrieve the title and link, and to clean up youtube titles for the goaztecscom yoututbe titles 
	def feed_parser 

		@sorted_feeds.each do |k,v|

			case 
			when v[1] == "rss"
				rss = SimpleRSS.parse open(v[0])
				rss.channel.items.each do |story|
					@tweet = "#{story.title} #{story.link}"
					send_tweet
				sleep(30)
				end 
			when v[1] == "atom"
				rss = SimpleRSS.parse open(v[0])
				rss.entries.each do |vid|
					title = vid.media_title.split('-')
					link = vid.link

					title.delete_at(-1)
					if title.length == 2
						message = "#{title[0].strip} #{title[1].strip}"
						@tweet = "#{message} #{link}" 
						send_tweet
						
					else 
					 	message = "#{title[0].strip}"
						@tweet = "#{message} #{link}"
						send_tweet
					end 
					sleep(20)
				end 
			end
		end 

	end 

	def send_tweet
		@client.update(@tweet)
		puts "Sent tweet: #{@tweet}"
	end 

end 

parser = RssParser.new('http://gdata.youtube.com/feeds/api/users/goaztecscom/uploads', 'http://www.utsandiego.com/rss/headlines/sports/sdsu-aztecs/')
parser.run
```
<h2>Next Steps</h2>
As mentioned there are some flaws with my code so I need to begin refactoring what I’ve got so far.  Below are my to-dos in order to improve this code so it will work with multiple feeds.
<ol>
	<li>This morning when I tried to rerun the code for some reason it threw an error that the youtube feed is a poorly formatted feed, so that will be the first thing to figure out.</li>
	<li>Store twitter keys in a yaml file and gitignore</li>
	<li>Figure out a better, more standard way of sorting the feeds</li>
	<li>Remove the case statement that changes the GoAztecs youtube titles</li> 
	<li>Add a check for whether or not the tweets are 140 chars.  If not reduce the title by n - 143 and append '...' to the truncated title and then tweet the title + the link</li>
	<li>Add a database with CRUD operations so I can store more feeds</li>
</ol>

<h2>Down the Road</h2>
Here are some things I'd like to do eventually with this little project.
<ol>
	<li>Turn it into a sinatra app (good way to learn Sinatra)</li>
	<li>Add a URL shortener or roll my own</li> 
	<li>Make it so a user can sign in and create their own RSS Twitter bot by providing their API keys</li>
</ol>






