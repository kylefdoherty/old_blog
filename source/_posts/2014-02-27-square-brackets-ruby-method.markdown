---
layout: post
title: ".[ ] Method...Huh?"
date: 2014-02-27 18:25:28 +0100
comments: true
categories: [Ruby, Ruby Methods]
---
I came across the blog post <a href="http://sirupsen.com/what-I-wish-a-ruby-programmer-had-told-me-one-year-ago/" target="_blank">“What I Wish a Ruby Programmer Had Told Me One Year Ago”</a> and started reading through the author Simon’s code for the first iteration of his todo app to figure out what was going on.  One method he used was:
```ruby
def [](id)
  @list[id]
end
```
This had me pretty confused at first. Then when I looked at how he was running the code I saw he was calling this method like you would access an element in an array.
```ruby
list = TodoList.load("todo.td")
list[0].done = true
```
It turns out in Ruby you can use special characters to name methods including the array lookup syntax, square brackets.  In this way he names the method <code>[ ]</code> and passes it an <code>(id)</code>, which is then passed to the array <code>@list</code> to lookup that element in the <code>@list</code> array.  Essentially he’s creating a lookup on the instance of his class <code>TodoList</code>, so he can use <code>[ ]</code> to lookup specific todo items just like he would if here were accessing them in array, which technically he is...the @list array.  Pretty Cool!

Check out this Stackoverflow answer for some good examples of how to use the [ ] method, <a href="http://stackoverflow.com/questions/10018900/how-does-defining-square-bracket-method-in-ruby-work" target="_blank">http://stackoverflow.com/questions/10018900/how-does-defining-square-bracket-method-in-ruby-work</a>

Here is another great Stackoverflow answer about Ruby Method names, <a href="http://stackoverflow.com/questions/10542354/what-are-the-restrictions-for-method-names-in-ruby" target="_blank">http://stackoverflow.com/questions/10542354/what-are-the-restrictions-for-method-names-in-ruby</a>



