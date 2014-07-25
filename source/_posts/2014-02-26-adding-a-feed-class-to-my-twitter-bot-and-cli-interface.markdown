---
layout: post
title: "Adding a Feed Class &amp; CLI Interface to my Twitter Bot "
date: 2014-02-26 12:47:41 +0100
comments: true
categories: Ruby
---

After reading the blog post <a href="http://sirupsen.com/what-I-wish-a-ruby-programmer-had-told-me-one-year-ago/" target="_blank">“What I Wish a Ruby Programmer Had Told Me One Year Ago”</a> and reading the code for his ToDo app I realized I should create a Feed class that has <a href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">CRUD</a> operations so I can list, add, edit, and delete my feeds.  It’s probably not the cleanest or most “ruby way" of doing it (if you have any suggestions please let me know) but it got the job done and I’ll refactor it to make it more rubish and/or turn it into a Sinatra app next.

```ruby
#class that stores feeds
class Feed
	attr_accessor :file

	def initialize(file = 'feeds.txt') # sets file to save feeds to, sets @feed array, and calls the list method 
		@file = file 
		@feeds = []
		
	end 

	def create # takes a feed from the user and adds it to the file 
		puts "Add feed"
		puts '>>'
		new_feed = gets.chomp
		File.open @file, 'a' do |f|
			f.puts new_feed
		end 
		puts "added the feed #{new_feed}"
	end

	def get_feeds #gets the feeds from the file and puts them in the @feeds array so they can be used by the .edit and .delete methods
		File.open(@file).readlines.each do |f|
			@feeds << f.strip 
		end 
	end 

	def write_feeds #writes the @feeds array to the file so .edit and .delete can write the @feeds array they've changed to the file 
		File.open @file, 'w' do |f|
			@feeds.each {|feed| f.puts(feed)}
		end
	end 

	def edit(name) #takes a file name from the user and then checks if that exists in the @feeds array. 
		get_feeds  #If it does it gets the id and asks the user what they want to change the name to and then changes that index to the new name.
				   #From here it calls the write_feeds method and writes the new array to the file 
		if @feeds.include?(name)
			index = @feeds.index(name)
			puts "What do you want to change #{name} to?"
			new_feed = gets.chomp
			@feeds[index] = new_feed
			write_feeds
		else 
			puts "no such feed"
		end 
		list
	end 


	def delete(name) #works almost the same as the .edit method but instead of changing the feed name it deletes it
		get_feeds

		if @feeds.include?(name) 
			index = @feeds.index(name)
			puts "Are you sure you want to delete #{name}?"
			puts "Enter 'yes' or 'no':"
			answer = gets.chomp.downcase
			if answer == "yes"
				@feeds.delete_if {|feed| feed == name}
				write_feeds
				puts "Feed #{name} has been deleted."
			else 
				puts "Ok I won't delete it then."
			end 
		else 
			puts "Sorry don't have that file."
		end 
	end 

	def list #if the feeds.txt file is empty it asks for feeds, if not it puts the feeds from the file 
		read_file = File.read @file
		if read_file.empty?
			puts "the file is empty please add some feeds"
			create
		else
			puts "here is the list of feeds from file #{@file}" 
			puts read_file
		end 
	end 


end 
```

After finishing this I decided I wanted to add some sort of command line interface so I could list out, add, edit, and delete the feeds from my terminal. I also added the -tweet command which runs the FeedParser class and begins tweeting out the feeds.  I realize there is an options parser library I could use for this but I wanted to design and build my own.

```ruby
require_relative 'feed'
require_relative 'feed_parser'

class CommandLineInterface #allows user to interact with feed.rb to list, add, edit, and delete 

	def initialize 
		@feeds = Feed.new
		take_commands
	end 

	def take_commands
		puts "What do you want to do?  To see list of commands enter -commands"
		until false 
		response = gets.chomp.downcase
		parse_response(response)
		end 
	end 

	def parse_response(response)
		if response == "-commands"
			puts "-list	  - list your feeds"
			puts "-add	  - add a new feed"
			puts "-edit	  - edit an existing feed"
			puts "-delete - delete an existing feed"
			puts "-tweet  - tweets out the feeds" 
			puts "-quit  - quit this program"

		else 
			case response 
			when "-list"
				@feeds.list
			when "-add"
				@feeds.add
			when "-edit"
				puts "feed name:"
				name = gets.chomp
				@feeds.edit(name)
			when "-delete"
				puts "feed name:"
				name = gets.chomp
				@feeds.delete(name)
			when "-tweet"
				parser = FeedParser.new(@feeds.get_feeds)
				parser.run
				
			when "-quit"
				exit 
			else 
				p 'not a valid command'
			end 
		end 
		take_commands
	end 
	 

end

cli = CommandLineInterface.new	
```
