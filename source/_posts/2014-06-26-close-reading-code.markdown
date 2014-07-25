---
layout: post
title: "Close Reading Code"
date: 2014-06-26 08:38:53 -0400
comments: true
categories: Ruby
---

##TL;DR
<strong>If you’re a rookie programmer like me your programming level is here:</strong>
<br>
<img class="center" src="/images/baby.jpg">
<strong>And you want to be at this level:</strong>
<br>
<img class="center" src="/images/ironic.jpg">

<strong>Use <a href="http://writingcenter.fas.harvard.edu/pages/how-do-close-reading" target="_blank">close reading techniques</a> to level up your coding skills.</strong>

##Learning to Code Well == Learning to Write Well
When I decided I wanted to become a programmer back in January I spent a lot of time thinking about the act of learning and what were the best ways to learn to program.  This meant I also spent a lot of time researching the topic on the interwebs and the advice I kept coming across was, <strong>“you need to write a lot of code and read even more.”</strong> This is when I started forcing myself to read other people’s code.

Then at the start of Flatiron School, Avi (the dean) described our coding ability one day as “Baby Talk.”  I thought this was a fair metaphor, but it also made me think about how learning to code well is like learning to write well.  One of the best ways to improve your writing skills is to use a technique called close reading, which transfers nicely to learning to program.

##WTF is Close Reading 
<a href="http://writingcenter.fas.harvard.edu/pages/how-do-close-reading" target="_blank">Close reading</a> is defined by <a href="http://en.wikipedia.org/wiki/Close_reading"target="_blank">wikipedia</a> as, "the careful, sustained interpretation of a brief passage of text. Such a reading places great emphasis on the single particular over the general, paying close attention to individual words, syntax, and the order in which sentences and ideas unfold as they are read." 

<img class="center" src="/images/close_reading.JPG">

Often close reading is used to better understand an academic text so you can compose an essay about it.  However, it can also be used to improve your reading and writing skills, because you’re paying close attention to vocabulary, sentence structure, style, etc. and begin to incorporate this into your own writing. This technique was especially helpful for me while studying in Chile and having to read academic papers in Spanish on topics such as the 1973 Arab–Israeli War and Business Operations.  At the time my Spanish was at a 7th grade level at best and these papers were 12 grade plus.  Thus, I spent a ton of time meticulously going through my assigned readings with pen, highlighter and Google to breakdown complex sentences, lookup words I didn’t know, and take notes detailed notes. This paid huge dividends down the road, helping me level up on my Spanish reading and writing skills...not that I still have them, but hey I did at one time.

##How to Close Read Code 
The way I close read code is similar to how I read an academic paper, except instead of paper and pen, I use my text editor and IRB.  While I’m reading through complex code I write comments above or next to each line, explaining in detail what is going on. And if I don't understand what's going on I just look it up in the Doc, throw it in IRB or use Pry, or I go to the Google.  With one or a combo of these you should be able to figure out what's goign on.

Here's an example of my detailed comments for a section of a Student class from a recent Flatiron lab:

``` ruby
class Student
  #setting the attributes of the student class to a class instance variable 
  ATTRIBUTES = { 
    :id => "INTEGER PRIMARY KEY",
    :name => "TEXT",
    :tagline => "TEXT",
    :github =>  "TEXT",
    :twitter =>  "TEXT",
    :blog_url =>  "TEXT",
    :image_url  => "TEXT",
    :biography =>  "TEXT"
  }
 
  # ATTRIBUTES.keys.each do |attribute|
  #   attr_accessor attribute
  # end
 
  #creating the attr_accessor by taking the keys from the ATTRIBUTES hash
  #which returns an array and sending that array to the attr_accessor 
  #method with the splat operator.  This 'explodes' the array into individual 
  #elements which are passed to the method
  attr_accessor *ATTRIBUTES.keys 
 
  #creates an empty students table by creating a SQL statment 
  #and assigning it to a variable and then passing that sql var to 
  #DB[:conn].execute. Inside the CREATE TABLE block
  #the method .schema_defininition is being called in order to insert the 
  #correct columns in the table (see schema definition below for explination)
  def self.create_table
    sql = <<-SQL
    CREATE TABLE IF NOT EXISTS #{self.table_name} (
        #{schema_definition}               
        )
    SQL
    DB[:conn].execute(sql)
  end
```

(All the <a href="https://gist.github.com/kylefdoherty/3f16da77103ff13b4c4d" target="_blank">code</a> for the Student class)

As you can see I try to explain exactly what is going on in detail so I'm aboslutely sure what's going on.

##Use the Code You Read
After you’ve read through and commented out what each line of code is doing, don’t stop there. I like to play with what I've learned in IRB to help me internalize it or rebuild it on my own.

For example, a week before Flatiron School started I learned a bit of Javascript and decided to try building a countdown to the first day of school. Google led me to this <a href"http://blog.smalldo.gs/2013/12/create-simple-countdown/" target="_blank">tutorial</a> which I disected line by line with comments for what was goign on.

```javascript
<h1 id='countdown-holder'>Coutndown goes here!</h1> 
 
<!-- creating an h1 tag wtih the id 'countdown-holder' 
so it can be targeted by JS with the .getElementById method-->
 
<script src="http://smalldo.gs/js/countdown.min.js"></script> 
 
 <!-- including the countdown.js script-->
 
<script>
  var clock = document.getElementById("countdown-holder") 
  
  //setting a variable named clock to 'document.getElementById' which is the method .getElementById being called on the 'document' (the html file). The id we're telling it to get is "countdown-holder"
  
  , targetDate = new Date(2014,05,02,09,00,00); 
  
  //setting a var named targetDate to a new date object with the date of June 2, 2015 at 9:00 am
 
  clock.innerHTML = countdown(targetDate).toString();
  
  // calling the .innerHTML method on clock to replace the h1 text with the countdown targetDate converted to a string
  
  setInterval(function(){
    clock.innerHTML = countdown(targetDate).toString();
  }), 1000;
  
  // the setInterval method executes a function over and over again, at specified time intervals. Here we're resetting the clock.innerHTML variable every second (1000 milliseconds in a second)
  
</script>
```

After getting a good understanding of what was going on, I tried coding it by myself and got this:

```javascript
<script src="http://smalldo.gs/js/countdown.min.js"></script> 
 
    <h1>Flatiron Ruby005 Countdown</h1>
    <h2 id='countdown-holder'>Countdown goes here!</h2> 
 
 
<script>
  var endDate = new Date(2014,05,02,09,00,00);
  var timeSpan = countdown(null, endDate );
  var clock = document.getElementById("countdown-holder");
 
  var timer = setInterval(function() {
    console.log(timeSpan = countdown(null, endDate ));
    if(timeSpan < 1)
    {
      clearInterval(timer); // need to find a way to set timespan at the global level so even after someone refreshes the page the timer doesn't run and the new clock text appears
 
    }
    console.log(clock.innerHTML = countdown(null, endDate ).toString());
  },1000);
</script>
```

This got me close to a working countdown script but wasn't quite working.  With a little help from <a href="https://twitter.com/ttsiege" target="_blank">Tristen</a> I was able to refactor my code and get it working.

```javascript
<script src="http://smalldo.gs/js/countdown.min.js"></script> 
 
  <h1>Flatiron Ruby005 Countdown</h1>
  <h2 id='countdown-holder'>Countdown goes here!</h2> 
 
<script>
  
  var timer = setInterval(function() {
    var endDate = new Date(2014,05,02,09,00,00),
      clock = document.getElementById("countdown-holder"),
      gif = document.getElementById("gif"),
      timeSpan = countdown(null, endDate);
 
    // breaks recursion
    if(timeSpan.value < 1)
    {
      clearInterval(timer); // need to find a way to set timespan at the global level so even after someone refreshes the page the timer doesn't run and the new clock text appears
      return clock.innerHTML = "Flatiron Ruby005 Has Begun!" 
    }
 
    clock.innerHTML = timeSpan.toString();
 
  },1000);
 
  
</script>
```
Close reading the original countdown code and then recoding it myself helped me grasp on a much deeper level how to declare JS variables, JS functions & function scope, and JS conventions, such as declaring multiple variables at once. 

##Final Thoughts
I know for me forcing myself to take the time to close read code is something I still struggle with, but I can honestly say this has helped me improve my code substantially. If you take the time to read code line by line, add detailed comments explaining what's going on, and then use it, it will pay off huge down the road.

##Resources on Learning & Learning to Program I like

* <a href="http://rubyrogues.com/131-rr-how-to-learn/" target="_blank">Ruby Rogues 131 - How to Learn</a>

* <a href="http://www.uvm.edu/~pdodds/files/papers/others/2007/ericsson2007a.pdf" target="_blank">HBR - The Making of an Expert</a>

* <a href="http://www.gamedev.net/blog/355/entry-2250592-become-a-good-programmer-in-six-really-hard-steps/" target="_blank">Become a Good Programmer in Six Really Hard Steps</a>

* <a href="http://catgrena.de/careers/2013/10/15/standing-out-from-the-crowd.html" target="_blank">Standing Our From the Crowd</a>

* <a href="http://rubyrogues.com/159-rr-hacking-education-with-saron-yitbarek/" target="_blank">Ruby Rogues 159 - Hacking Education with Saron Yitbarek (Flatiron School Alum)</a>

* <a href="https://www.youtube.com/watch?v=mW_xKGUKLpk" target="_blank">Reading Code Good - Saron's RailsConf2014 Talk</a> 







