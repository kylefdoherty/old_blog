---
layout: post
title: "Recoding My RSS Twitter Bot"
date: 2014-02-19 12:03:44 +0100
comments: true
categories: Ruby 
---

My last blog post about my RSS Twitter Bot made realize a lot of flaws with my bot, not only in the code I wrote but the choices I made about what to code.  This led me to re-code the entire bot, but I think the code is much cleaner now.
<!-- more -->
```ruby  Recoded RSS Twitter Bot 
require 'rss'
require 'open-uri'
require 'twitter'
require 'yaml'

class FeedParser 
	def initialize(*feeds)
		@feeds = feeds 
		@tweet = ""
		@pub_date = ""
		@tweet_length = Proc.new{ 
			if @tweet.length > 140
				next 
			else 
				tweeter(@tweet)
				puts "tweeted"

				sleep(15)
			end 
		}

		config = YAML.load_file('config.yml')

		@client = Twitter::REST::Client.new({
  			consumer_key: config['consumer_key'],
  			consumer_secret: config['consumer_secret'],
  			access_token: config['access_token'],
  			access_token_secret: config['access_token_secret']
		})
		
	end 

	def run 
		feed_sorter
	end 

# method that sorts feeds
	def feed_sorter
		@feeds.each do |url|
			open(url) do |rss|
  			feed = RSS::Parser.parse(rss)
			
				case 
				when feed.feed_type == "rss"
					rss_parser(feed)
				when feed.feed_type == "atom"
					atom_parser(feed) 
				end 
			end 
		end 
	end 

	def rss_parser(rss)
		rss.items.each do |item| 
			title = item.title
			link = item.link
			@tweet = "#{title} #{link}"
			@tweet_length.call 
			
		end 
	end 


	def atom_parser(atom)
		atom.items.each do |item| 
			title = item.title.content
			link = item.link.href
			@tweet = "#{title} #{link}"
			@tweet_length.call

		end 
	end 

	def tweeter(tweet)
		@client.update(tweet)
	end 
end 


parser = FeedParser.new('https://gdata.youtube.com/feeds/api/users/goaztecscom/uploads', 'http://www.utsandiego.com/rss/headlines/sports/sdsu-aztecs/')
parser.run
```

<h2>I Switched to the RSS Module</h2>
During and after writing the first post about my Twitter Bot I did more research into the Ruby RSS Module and found that it has <code>.feed_type</code> method that allows you to easily sort the types of feeds so you can use the correct methods to parse them.  Sorting the feeds this way is infinitely better than the weird/incorrect way I was doing it before albeit it was good practice with hashes.

```ruby Sorting Feeds with the RSS Module
@feeds.each do |url|
	open(url) do |rss|
  	feed = RSS::Parser.parse(rss)
			
		case 
		when feed.feed_type == "rss"
			rss_parser(feed)
		when feed.feed_type == "atom"
			atom_parser(feed) 
		end 
	end 
end
```
<h2>I Finally Used a Proc in my Code</h2>
I remember learning about Procs and Lamdas on Codecademy and thinking they were pretty cool...ok really cool.  Since that time I haven’t used them in my code, accept when trying to code FizzBuzz in as many different ways as possible, which sort of makes sense since everything I’m doing is super simple, but still...I want to use a some damn Procs.

Finally, I found a reason.  When re-coding my bot I decided there was no reason to try and adjust the length of the feed titles because eventually my bot will hopefully use a URL shortener, meaning 99.99% of the time the titles will never make the tweet go over 140 characters.  Still I needed an easy way to keep <code>@tweets</code> that are over 140 characters from being sent to the <code>.tweeter</code> method and causing an error.  Obviously, I could stick an if/else statement in a method and call it from the <.rss> and <.atom> methods, but using a Proc sounded way more fun :).

```ruby Using a Proc
@tweet_length = Proc.new{ 
if @tweet.length > 140
		next 
	else 
		tweeter(@tweet)
		puts "tweeted"
		sleep(15)
	end 
}
```

<h2>Next Steps</h2>
<ol>
  <li>Check for new articles, videos, etc. in the feeds and only tweet the new ones</li>
  <li>Learn Sinatra and turn this into a Sinatra App</li>
  <li>Add a url shortner like bit.ly or roll my own</li>
</ol>
