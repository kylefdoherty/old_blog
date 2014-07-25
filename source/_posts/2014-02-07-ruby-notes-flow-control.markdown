---
layout: post
title: "Ruby Notes - Control Flow"
date: 2014-02-07 10:52:33 +0100
comments: true
sharing: true
categories: [Ruby, Control Flow]
---


I’m 4 weeks into my 25 hours a week of Ruby self-study (I work part time right now otherwise it’d be 40+ hours) and just completed 
<a href="http://www.codecademy.com/tracks/ruby" target="_blank">Codecademy’s Ruby track.</a>  Finishing was great but I felt I'd  only scratched the surface of Ruby and programming in general.  To supplement my minimal Ruby knowledge I decided to begin additional research and create study notes and do practice problems for each topic covered by Codecadmny, such as control flow, arrays, blocks, etc.  This has already proven a success as I’ve seen a marked difference in my confidence and coding ability after completing notes for Control Flow and Arrays.  
<!-- more -->
Now I’ve decided to turn my notes and practice problems into blog posts because:

<ol> 
  <li>Hopefully it will be helpful to other people learning Ruby</li>
  <li>It forces me to read through my notes and practice problems again</li>
  <li>It requires rewriting my notes in way that others can understand thus further driving the concepts home for me</li>
  <li>Hopefully I’ll get feedback on my notes and practice problems (hint hint)</li>
</ol>

<h2>Control Flow</h2>

Control flow allows you to control how your ruby script executes based on conditions.  For example, <code>if</code> condition A is met then do B OR <code>unless</code> B happens do A.  

<h2>If Statement</h2>
An if statement tells the program what to do if a condition is met. That is, if the conditional that comes after <code>if</code> is true then execute the following code. If it evaluates to false, it doesn’t execute.

```ruby Example 1 - If Statements
money = true

if money 
  puts “You have money.”
end 
```

This if statement is saying if the variable money is true, puts the message “You have money.”  In this case if money were set to false, nothing will puts to the screen.

```ruby Example 2 - If Statements 
if 2 < 3
  puts “2 is less than 3”
end 
```

Since 2 is less than 3 this if statement evaluates to true and thus puts’ “2 is less than 3”.  If we had typed <code>if 2 > 3</code>, puts would not have been executed and ‘nil’ would have been returned because the if statement would have evaluated to false.

<h2>If/Else Statement</h2>
Adding an else into the if statement essentially creates a default i.e. in the event whatever comes after <code>if</code> is false then do this.


```ruby Example - If/Else Statement
password = 12345678

if  password.length >= 8 
	puts “your password is long enough”
else 
	puts “your password is too short”
end 
```

<h2>If/Elsif/Else</h2>
Using an elsif allows for multiple conditions to be added to the if statement.  In this way we can essentially add multiple if statements to check for many different possible conditions.

```ruby Example - If/Elsif/Else Statement 
city = “San Francisco”

if city == “New York”
  puts “Jets & Giants”
elsif city == “Denver”
  puts “Broncos”
elsif city == “San Francisco”
  puts “49ers”
else 
  puts “Hmm we don’t know what NFL team that city has.”
end 
```

If the variable <code>city</code> is set to any of the cities in our If/Else statement, the program will puts what NFL team that city has.  If <code>city</code> is not set to one of those cities then the program puts the default message, "Hmm we don’t know what NFL team that city has.”

<h2>Unless</h2>
Unless is the opposite of an if statement.  An if statement only executes if the conditional is true, conversely unless only executes if the conditional is false.  It’s good practice to use unless instead of using <code>if !=</code> (if not equal to) since <code>if !=</code> can be a bit confusing.

The following if statement can be replaced with an unless statement.

```ruby Example of Bad If Statement
puts "Give us a message."
message = gets.chomp

if message != “”
  puts “#{message}
end 
```

By using unless this piece of code becomes much more readable.

```ruby Example of When to Use Unless
puts "Give us a message."
message = gets.chomp

unless message.empty? 
  puts "#{message}"
end 
```

Another good use case for Unless is to use them in a single line like so:

```ruby Example 2 - When to Use Unless 
milk = false

puts “We’re out of milk.” unless milk  

#=> We’re out of milk
```

Here we’re saying only puts we’re out of milk if the milk variable is set to false.  Since in this case milk is set to false then the message would be shown to the screen.

One more example:

```ruby Example 3 - When to Use Unless
i += 1 unless i > 10
```

Here <code>i</code> is being increased by 1 unless it is more than 10.

<h3>Unless Best Practices</h3>
There are some best practices you should follow when using unless to ensure you don’t confuse people or yourself.  These aren’t universal, especially considering the Codecademy section on control flow doesn’t follow all of them, but after doing some research I’ve found these best practices given several places such as <a href="https://www.codeschool.com/courses/ruby-bits" target="_blank">Code School’s Ruby Code Bits course</a> and a blog post on the 37Signals blog called <a href="http://signalvnoise.com/posts/2699-making-sense-with-rubys-unless" target="_blank">"Making Sense with Ruby’s ‘Unless’”</a>.

<ol>
	<li>Don’t use else with unless.  Codeacademy does this in some of their examples with unless and they aren’t all that confusing but they aren’t that straightforward either. It’s probably better to use an if statement if you want to use else.</li>
	<li>Avoid using more than a single logical condition.  For example, doing things like <code>unless a && b</code> can become confusing fast.</li>
	<li>No double negatives.  Unless is already a negative so don’t add in a != to make it a double negative...that’s just bad english.</li>
</ol>

<h2>Inline Conditionals</h2>
As mentioned when discussing unless, sometimes it’s best to write an if or unless statement on 1 line.  Here are some examples:

```ruby Multy Line If Statement
if zipcode < 5
  fail “zipcode too short”
end 
```

Can become:

```ruby Single Line If Statement 
fail “zipcode too short” if zipcode.length < 5
```

Some more examples:

```ruby Single Line If Statement 
puts “Input your name:” if name.empty?
```
```ruby Single Line Unless Statement 
puts fail “no username set” unless username
```
<h2>The Ruby Ternary Operator</h2>
The ternary operator is another way to write an if else statement.  The word ternary means consisting of or involving three. The ruby ternary operator consists of the following 3 parts:

condition ? value if true : value if false

In this case the ? is analogous to the word THEN and the : is analogous to OR.  What a ternary operator is saying is if the condition evaluates to true THEN return the value if true OR if the condition evaluates to false, output the value if false.

This may not make sense 100% but an example should shore that up.

```ruby Ternary Operator Example 
milk = true

puts milk ? “We have milk” : “We don’t have milk”
```

Here the ternary operator is saying if milk = true THEN puts “We have milk” OR if milk = false, puts “We don’t have milk.” 

For simple if/else statements Ruby’s ternary operator can be a nice thing to use.

<h2>Case Statements</h2>
Sometimes using an if/else statement can be too wordy and that is when you need a case statement.  Instead of saying if, elsif, else, you simply use when, which is saying, when the conditional is true, run this code.

Here’s an example:

```ruby Case statement example 
animal = “dog”

case animal 
when animal == “cat”
  puts “Meow!”
when animal == “parrot”
  puts “Polly want a cracker!”
when animal == “dog”
  puts “Woof!”
else 
  puts “What animal is that?”
end 
```   

Further reading on sase statements try - <a href="http://www.skorks.com/2009/08/how-a-ruby-case-statement-works-and-what-you-can-do-with-it" target="_blank">How A Ruby Case Statement Works and What You Can Do With It</a>



