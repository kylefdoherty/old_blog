---
layout: post
title: "Codeacademy Redactor Problem - Think About The Following..."
date: 2014-01-30 10:45:04 +0100
comments: true
categories: [Codeacademy, Ruby]
---
At the end of the Redacted! project in Codeacademy’s Ruby track they ask you to think about a few things:

What could you do to make sure your redactor redacts a word regardless of whether it's uppercase or lowercase?
How could you make your program take multiple, separate words to REDACT?
How might you make a new redacted string and save it as a variable, rather than just printing it to the console?
Below I’ve shown how I solved each of these so other people doing this problem can see how I solved it, critique my code, and share their solutions. 
<!-- more -->


As a starting point here is my code for the initial Redacted project before I began solving for the above questions.


``` ruby  Original code:
puts "block of text:"
text = gets.chomp

puts "word to be redacted:"
redact = gets.chomp

words = text.split(" ")

words.each do |word|
    if word == redact 
        print "REDACTED "
    else
        print word + " " 
    end
end 
```

<h2>Question #1 - What could you do to make sure your redactor redacts a word regardless of whether it's uppercase or lowercase?</h2>

To achieve this I figured I had two options.  Either downcase (or upcase) the block of text and the word(s) to be redacted given by the user as soon as they’re given by the user or do this when they are being compared.  Below are both ways.

``` ruby When given by user:
puts "block of text:"
text = gets.downcase.chomp

puts "word to be redacted:"
redact = gets.downcase.chomp

words = text.split(" ")

words.each do |word|
    if word == redact 
        print "REDACTED "
    else
        print word + " " 
    end
end 
```




``` ruby When comparing:
puts "block of text:"
text = gets.chomp

puts "word to be redacted:"
redact = gets.chomp

words = text.split(" ")

words.each do |word|
    if word.downcase == redact.downcase 
        print "REDACTED "
    else
        print word + " " 
    end
end 
```

<h2>Question #2 - How could you make your program take multiple, separate words to REDACT?</h2>

First I need to make it so the user can input as many words to be redacted as they want.  To achieve this I used a while loop and an if statement.  If the user inputs a keyword signifying they are done inputting words to be redacted the while loop breaks.  

``` ruby
while input = gets.chomp
  if input.downcase == "done"
      break 
  else 
      redact << input.downcase
  end 
end 
```

Here I make “done” the keyword that breaks the while loop.


Next, I needed to figure out how to compare the two arrays, redact and words.  Funny enough after a quick search on comparing two arrays I found out that using & is one way to compare two arrays and see what objects they have in common (http://blog.hulihanapplications.com/browse/view/28-ruby-array-comparison-tricks).  So if you write redact & words the output will be the objects they have in common.  I thought this was interesting but didn’t really help me because it basically just recreated my redact array. 
Next I broke down what I need to do in plain english, hoping that would help me break down the problem and figure out how to solve it with the little ruby I know and myabe a little help from Google.
Breaking it Down
The first thing I know I needed to do was loop through the words array with .each and then see if that word matches any words in the redact array.  So effectively what I’m doing is comparing a string to an array.  With a google search of “compare a string to an array ruby” I found that there is an .include? method for arrays that does exactly this (http://stackoverflow.com/questions/3770316/how-to-compare-a-string-against-multiple-other-strings).  It works by calling the .include? method on the array and passing it a string. It then checks to see if that string is included in the array.  PERFECT!

Here is what it looks like in code:

``` ruby
words.each do |word|
  if redact.include? word
    print "REDACTED "
  else
    print word + " "
  end
end
```

And here is the entire script:

``` ruby
redact = []
puts "block of text:"
text = gets.downcase.chomp

words = text.split(" ")

puts "Give as many words to be redacted as you want."
puts "Once you are done type \'done\'."
puts "Please give the words you want redacted:"

while input = gets.chomp
  if input.downcase == "done"
      break 
  else 
      redact << input.downcase
  end 
end 

#common_words = redact & words

words.each do |word|
  if redact.include? word
    print "REDACTED "
  else
    print word + " "
  end
end
```

<h2>Question #3 - How might you make a new redacted string and save it as a variable, rather than just printing it to the console?</h2>

I skipped this question because it didn’t seem to make sense for my script.  Since I allow the user to input multiple strings to redact that are then saved in the array redact there seems to be no reason to save a new redacted string.  What would you do with it?
If you have a better sense of what they are asking here please let me know.