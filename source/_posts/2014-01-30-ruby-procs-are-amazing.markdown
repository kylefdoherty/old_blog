---
layout: post
title: "Ruby Procs are Amazing!"
date: 2014-01-30 15:12:44 +0100
comments: true
categories: [Ruby, Codeacademy, Procs]
---

I just got through the <a href="http://www.codecademy.com/courses/ruby-beginner-en-L3ZCI/1/1?curriculum_id=5059f8619189a5000201fbcb" target="_blank">Codeacademy Procs section</a> and got so excited I tweeted about them and emailed my girlfriend explaining what Procs are and why they’re so cool...bad I know.  Luckily she works in finance and is a total excel geek so she can empathize and won’t think I’m a complete weirdo.
<!-- more -->
{% img center /images/proc_tweet.png 'Proc Tweet' 'Tweet about how cool Ruby Procs are' %}

{% img center /images/proc_email.png 'Proc Email' 'Email about how cool Ruby Procs are' %}

Since I already tweeted and sent an email about Procs I figured I’d post about them to round things out.

<h2>Why Procs are Cool</h2>

Procs allow you to save a block of code, something like <code>{ |x| x *3 }</code> which is saying take whatever I give you (x) and multiply it by 3.  What you can then do with this is pass it to methods.

So for example, I have an array of people's heights, like so:

``` ruby
array = [4.5, 5, 6, 3.5, 3]
```

And I want to get all the heights that are above 4 feet tall because that's how tall you have to be to ride the ride...le duh.

I could write some long method called <code>rubyover_4_feet</code> where I pass it the array and then it returns the heights that are above 4 feet tall.  But that would require 9 lines of code...maybe less but that's how many lines it took me.

``` ruby
def over_4_feet(array)
    new_arr = []
    array.each do |i|
        unless i < 4
            new_arr << i
        end 
    end 
    return new_arr
end 
```

Instead I can call the method <code>.select</code> on the array and pass it a Proc that tells it to only select heights that are taller than 4 feet.

Below is a proc that checks for being over 4 feet tall. Then I just pass that proc to the <code>.select</code> method telling it to only "select" heights over 4 feet.

``` ruby
over_4_feet = Proc.new { |x| x >= 4 }

array.select(&over_4_feet)
```

So what took me 9 lines of code I did in 1, since passing the Proc to select is the same as calling the over_4_feet method.

<h3>AMAZING!!!</h3>
<a href="http://gifsoup.com/view/2169429/nets-fan.html" target="_blank"><img src="http://stream1.gifsoup.com/view3/2169429/nets-fan-o.gif" border="0"/></a><br /><a href="http://gifsoup.com" title="GIFSoup" target="_blank"></a>







































