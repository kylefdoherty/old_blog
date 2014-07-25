---
layout: post
title: "Ruby Inject Basics"
date: 2014-06-13 00:06:01 -0400
comments: true
categories: 
---


This week during lecture at Flatiron school, <a href="https://twitter.com/aviflombaum" target="_blank">Avi</a> showed us Ruby’s <code>.inject</code> method (also known as .reduce).  I thought this seemed like a pretty cool and potentially powerful method and decided to do a bit more research so I could start using it with confidence in my code.

##How Inject Works
<code>.inject</code> is an iterator method (technically an enumerable method) that creates an accumulator variable that “accumulates” whatever you want as you iterate over some sort of collection and returns the accumulator when the iteration is complete.

Instead of trying to explain further let’s take a peek at a simple example.  

```ruby 
[1,2,3,4].inject do |accumulator, element|

  puts "accumulator: #{accumulator}, element: #{element} - 
  Adding them: #{accumulator} +  #{element} = #{accumulator + element}"

  accumulator + element 

end

#=> accumulator: 1, element: 2 - Adding them: 1 +  2 = 3
#=> accumulator: 3, element: 3 - Adding them: 3 +  3 = 6
#=> accumulator: 6, element: 4 - Adding them: 6 +  4 = 10
#=>  => 10 

```

Here we can see we’re iterating over the array and adding the element to the accumulator each time through.  If you look at the output from the puts you can see that the accumulator starts at 1, the first element in the array, and the element variable is given the 2nd element of the array.  As it iterates through the array, the sum becomes the accumulator’s value and then is returned at the end of the iteration.

Here’s that same example but this time we’re multiplying the elements in the array.

```ruby
[1,2,3,4].inject do |accumulator, element|

  puts "accumulator: #{accumulator}, element: #{element} - 
  Mulitplying them:#{accumulator} *  #{element} = #{accumulator * element}" 

  accumulator * element 
end

#=> accumulator: 1, element: 2 - Mulitplying them: 1 *  2 = 2
#=> accumulator: 2, element: 3 - Mulitplying them: 2 *  3 = 6
#=> accumulator: 6, element: 4 - Mulitplying them: 6 *  4 = 24
#=>  => 24 
```

##Giving the Accumulator a Starting Value 
In the last two examples we’re not giving the accumulator a starting value, however we have the option to do this by passing the value to the .inject method. 

```ruby 
[1,2,3,4].inject(0) do |accumulator, element|

  puts "accumulator: #{accumulator}, element: #{element} - 
  Adding them: #{accumulator} +  #{element} = #{accumulator + element}"

  accumulator + element 

end

#=> accumulator: 0, element: 1 - Adding them: 0 +  1 = 1
#=> accumulator: 1, element: 2 - Adding them: 1 +  2 = 3
#=> accumulator: 3, element: 3 - Adding them: 3 +  3 = 6
#=> accumulator: 6, element: 4 - Adding them: 6 +  4 = 10
#=>  => 10 

```
This time we’re summing the contents of the array, like in the first example, but we’re giving the inject method a value to set the accumulator to.  Here we’ve set it to 0, but you can set it to anything you want.  You can see from the output that the accumulator starts at 0 and the element variable is set to the first element in the array, 1.

But why do we want to set the accumulator to 0?  This is just making us loop through one additional time?

For this summing example, this is true but if we we’re doing something to the element on each iteration, like say incrementing it by two, we need to give the accumulator variable a starting value.  If we don’t then it uses the first element in the array, we can’t increment it, and thus don’t get the return value we want.

Here’s a look at this:

```ruby

##### Giving the Accumulator a Starting Value #####

[1,2,3,4].inject(0) do |accumulator, element|

  puts "accumulator: #{accumulator}, element + 2: #{element + 2} - Adding them: #{accumulator} +  #{element + 2} = #{accumulator + (element +2)}"

  accumulator + (element + 2) 

end

#=> accumulator: 0,   (element + 2):  3 - Adding them:   0 +  3 = 3
#=> accumulator: 3,   (element + 2):  4 - Adding them:   3 +  4 = 7
#=> accumulator: 7,   (element + 2):  5 - Adding them:   7 +  5 = 12
#=> accumulator: 12, (element + 2):  6 - Adding them: 12 +  6 = 18
#=> =>18


##### Omitting a Starting Value #####

[1,2,3,4].inject do |accumulator, element|

  puts "accumulator: #{accumulator}, element + 2: #{element + 2} - Adding them: #{accumulator} +  #{element + 2} = #{accumulator + (element +2)}"

  accumulator + (element + 2) 

end

#=> accumulator: 1,   (element + 2):  4 - Adding them:   1 +  4 = 5
#=> accumulator: 5,   (element + 2):  5 - Adding them:   5 +  5 = 10
#=> accumulator: 10, (element + 2):  6 - Adding them: 10 +  6 = 16
#=> =>16

```

You can see that if we don’t set the accumulator to 0 we don’t increment the first element by 2 and thus get 16 when we really wanted 18.  Because of this, it’s probably a good idea to get in the habit of setting the accumulator value no matter what.


##Altering Hashes 
Another use case for .inject (.reduce) is if you need to change the key or value in a hash. 

```ruby

hash_of_state_capitals = {"CA" => "Sacramento", "NY" => "Albany", "ND" => "Bismarck"}


hash_of_state_capitals = hash_of_state_capitals.reduce({}) do |new_hash, (state, capital)|

  new_hash[state.to_sym] = capital
  
  new_hash
end 

#=>  {:CA=>"Sacramento", :NY=>"Albany", :ND=>"Bismarck"}

```
In this example, we have a <code>hash_of_state_capitals</code> whose keys are strings, and we want to change them to symbols.  

This time around, instead of setting the accumulator to a number we’re setting it to an empty hash ( <code>hash_of_state_capitals.reduce({})</code> ), which we’re calling “new_hash”.  Then each time we loop through the hash_of_state_capitals we create the new_hash by setting the key to the <code>state.to_sym</code> and the value to capital.  Then at the bottom we return the new hash, which is a must, and will throw an error if you don’t. 

This method of altering a hash is a bit cleaner than using <code>.each</code> and can actually be done on a single line.

```ruby

hash_of_state_capitals = hash_of_state_capitals.reduce({})  {|new_hash, (state, capital)| new_hash[state.to_sym] = capital; new_hash}

### OR ###

hash_of_state_capitals = hash_of_state_capitals.reduce({})  {|new_hash, (state, capital)| new_hash.merge(state.to_sym => capital)}


```

##Building Arrays & Hashes
You can use inject to build arrays and hashes similar to how you use other methods such as .map or .select.  The advantage that inject has over these is you can use inject instead of chaining several methods together. This isn’t a big deal if you’re chaining just two methods together to achieve whatever you’re doing but your code could get messy if you go beyond that.  

Here is a great example from <a href= "http://blog.jayfields.com/2008/03/ruby-inject.html" target="_blank">Jay Fields’ blog post</a> on Inject demonstrating how to use <code>.inject</code> to get all the integers of an array, that are even, as strings.


```ruby 

### Chaining Select & Collect ###
[1, 2, 3, 4, 5, 6].select { |element| element % 2 == 0 }.collect { |element| element.to_s } 

# => ["2", "4", "6"]


### Using Inject ###
array = [1, 2, 3, 4, 5, 6].inject([]) do |result, element|
 result << element.to_s if element % 2 == 0
 result
end

# => array =  ["2", "4", "6"]

```

An example of building a hash would be if you received an array with nested arrays, that you wanted to convert into a hash as key, value pairs.

```ruby 

students = [[01, "Jon"],[02, "Becca"], [03, "Pete"], [04, "Laura"]]

students_hash = students.reduce({}) do |students, array| 

  students[array.first] = array.last
  
  students
end 

#=> {1=>"Jon", 2=>"Becca", 3=>"Pete", 4=>"Laura"}

``` 
Here we have student’s IDs and names given to us as an array of arrays, but we want to convert this to a hash.  The inject method takes care of this in a nice compact way, without having to create empty variables outside our loop.

Finally here is an example from the anagram_detector lab we did today.

```ruby
anamgrams = %w(gallery ballerina regally clergy largely leading)

word = 'allergy'


class Anagram 
  attr_accessor :word

  def initialize(word)
    self.word = word
  end 

  def word_sorter(word)
    word.chars.sort.join("")
  end 


  def match(array)
    array.inject([]) do |array, element|
      array << element if word_sorter(element) == word_sorter(word) 
      array
    end 
  end 

  # def match(array)
  #   array.select do |element|
  #     word_sorter(element) == word_sorter(word)
  #   end 
  # end 

end

a = Anagram.new(word)
puts a.match(anamgrams)

#=> ["gallery", "regally", "largely"]

```

You can see in my commented out code that you can do the same thing with the <code>.select</code> method but I think inject is almost as elegant and gives us the ability to do additional things that <code>.select</code> can’t.  Such as, build a hash with the returned strings or manipulate the strings further.

##That’s All Folks
And that’s all I’ve got on Ruby’s Inject for now.  From what I’ve read it seems <code>.inject</code> becomes even more powerful when you start using it with objects but that’s for another blog post.  To learn more below are links to all the blog posts I referenced for writing this post and I suggest reading them if you want to learn more.

##Additional Resources:

* <a href= "http://blog.jayfields.com/2008/03/ruby-inject.html" target="_blank">Ruby Inject - Jay Fields’ Thoughts</a>

* <a href="http://www.sitepoint.com/guide-ruby-collections-iii-enumerable-enumerator/"" target="_blank">A Guide to Ruby Collections III: Enumerable and Enumerator - Sitepoint</a>

* <a href="http://blog.teamtreehouse.com/ruby-arrays" target="_blank">The Basics of Ruby Arrays - Team Treehouse Blog</a>

* <a href="http://chrisholtz.com/blog/lets-make-a-ruby-hash-map-method-that-returns-a-hash-instead-of-an-array/"" target="_blank">Let's Make a Ruby Hash Map Method That Returns a Hash Instead of an Array</a>


