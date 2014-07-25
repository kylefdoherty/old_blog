---
layout: post
title: "Getting and Parsing the Citi Bike JSON Feed with HTTParty"
date: 2014-06-30 22:02:46 -0400
comments: true
categories: Ruby, JSON, HTTParty, Citi Bike 
---
##BigIdeas Citi Bike Finder Challenge 
For our Flatiron School Meetup Presentation, <a href="http://amyrjohnson.github.io/" target="_blank">Amy Johnson</a> and I are trying to tackle at least part of one of NYCBigApps’ BigIdeas Challenges posted by Citi Bike, <a href="http://citibikefinder.splashthat.com/" target="_blank">http://citibikefinder.splashthat.com/</a>.  The main problem they outline is that during high commute times people often find themselves being “dockblocked” or “bluebiked”, as the <a href="http://gothamist.com/2013/06/27/citibike_fail_dumbo_totally_dockblo.php" target="_blank">Gothamist put it</a>, because there aren’t any available spaces to deposit their bike.  

The main idea for a solution is to have some sort of suggestion app for starting and ending locations with estimated availability based on historical data.  There are some other features they have on their wishlist for the app, but we figure this alone is about all we want to try to bite off for the moment.

##Our Potential Solution 
Obviously the first thing we need to figure out is, is what data is available to us. It seems, for the moment at least, that there isn’t historical data on bike dock stations readily available to us (we’ve reached out and are waiting to hear back on this), so for the time being it looks like we’ll potentially need to hit the real time <a href="http://www.citibikenyc.com/stations/json" target="_blank">JSON feed</a>, and gather data that way.  Plus even if there is historical data we can get we still may want to collect our own and we’ll definitely want to be grabbing data about station’s current available bikes and docks, if they’re in service, etc.

##It’s a HTTParty
The first to do then is figure out how to get and parse this JSON feed. JSON by the way stands for <a href="http://en.wikipedia.org/wiki/JSON" target="_blank">JavaScript Object Notation</a> and gives you back data in attribute-value pairs...basically a hash.  After a quick search on the Google I found the HTTParty gem, which is a HTTP client gem that allows you to easily make HTTP requests as well as automatically parses JSON or XML responses you get back.

##Ok down to business...Parsing the Citi Bike Feed.
This is super simple with HTTParty.  All you have to do is require the gem and then use <code>HTTParty.get(“the_url”)</code>, and if you stick pry in your file it becomes super duper easy to play with the <code>HTTParty::Response</code> object that is returned to you.

```ruby 
require 'httparty'
require 'pry'


response = HTTParty.get("http://www.citibikenyc.com/stations/json")

binding.pry
```
What you get back is technically, as mentioned, a <code>HTTParty::Response</code> object, but really it’s a hash.  So if we call <code>.keys</code> on it we can see what keys it has.

```ruby 
response.keys
# => ["executionTime", "stationBeanList"]
```
My guess is “stationBeanList” is what we want and sure enough it gives us back a big array of 332 station hashes.

```ruby 
response["stationBeanList"]
  
# => [{"id"=>72,
 #  "stationName"=>"W 52 St & 11 Ave",
 #  "availableDocks"=>19,
 #  "totalDocks"=>38,
 #  "latitude"=>40.76727216,
 #  "longitude"=>-73.99392888,
 #  "statusValue"=>"In Service",
 #  "statusKey"=>1,
 #  "availableBikes"=>13,
 #  "stAddress1"=>"W 52 St & 11 Ave",
 #  "stAddress2"=>"",
 #  "city"=>"",
 #  "postalCode"=>"",
 #  "location"=>"",
 #  "altitude"=>"",
 #  "testStation"=>false,
 #  "lastCommunicationTime"=>nil,
 #  "landMark"=>""},
 # {"id"=>79,
 #  "stationName"=>"Franklin St & W Broadway",
 #  "availableDocks"=>32,
 #  "totalDocks"=>33,
 #  "latitude"=>40.71911552,
 #  "longitude"=>-74.00666661,
 #  "statusValue"=>"In Service",
 #  "statusKey"=>1,
 #  "availableBikes"=>1,
 #  .....
```
From here it’s easy to grab specific data out of this array of stations.  For example, the stations whose “statusKey” does not equal 1 since that probably will mean they’re not in service and might show me the other possible status keys.

```ruby 
response["stationBeanList"].select {|station| station["statusKey"] != 1}}

  # => returns array of 5 station hashes with “statusKey” => 3 and “statusValue” => “Not in Service
```

Or let’s say we want to see the stations that have over 50 docks i.e the big ass docks in the city.

```ruby 
 big_ass_docks = response["stationBeanList"].select {|station| station["totalDocks"] >= 50}

 # => An array of 32 station hashes 
```

##Next Steps
Getting the JSON feed and parsing it was way simpler than I had imagined. From here we need to figure out what our absolute bare bones app needs to do and then plan from there. We can also for sure create a Station class and generate a bunch of Station objects from the stations array.  This way anytime we hit the JSON feed we can have these objects update their attributes. 
