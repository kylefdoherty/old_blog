---
layout: post
title: "Getting Started with JavaScript and Google Maps API"
date: 2014-07-17 11:56:31 -0400
comments: true
categories: 
---

{Intro to what I did and how this blog post will mostly be code with comments & examples of what it looks like from codepen that you can then fork and play with it yourself.}

##Centering a Map 
The first thing you’re going to want to do is create a map object and set the center of the map and zoom level.  The Google Maps API provides a nice <a href="https://developers.google.com/maps/documentation/javascript/examples/map-simple" target='_blank'>code sample</a> for making a simple map, which isn’t too difficult to figure out even if you’re a JS noob like me, but to really understand what everything was doing I commented out what each line of HTML & JS is doing.

```javascript 
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
    <style type="text/css">
    /* simple styling for the webpage and 
    specifically the #map-canvas id */
      html { height: 100% }
      body { height: 100%; margin: 0; padding: 0 }
      #map-canvas { height: 100% } /*BE SURE TO - set the
      height of the map-canvas div */
    </style>
    <script type="text/javascript"
      /*uses the google maps api with your api key, 
      https://developers.google.com/maps/documentation/
      javascript/tutorial#api_key*/
      src="https://maps.googleapis.com/maps/api/js?key=API_KEY">
    </script>
    <script type="text/javascript">
    // creates the funtion initialize which is called at the bottom of the script.
      function initialize() {
        /*creates a mapOptions object literal with a center
        and zoom level. A JS object literal looks a lot like 
        a hash in Ruby.  Here is a blog post describing the
        difference, http://stantona.github.io/blog/2012/12/12/a
        -tale-of-two-object-models-javascript-and-ruby/*/
        var mapOptions = {
          /*the center key sets the center of the map to the
          specified latitude and longitude. It's currently set
          to be centered on Manhattan*/
          center: new google.maps.LatLng(40.7903, -73.9597),
          /*zoom key sets how zoomed in or out you are, with 0
          being the farthest zoomed out and 20 being the 
          most zoomed in*/
          zoom: 8
        }; //end of var mapOptions

        /*sets the var map to a new instance of a google map 
        and injects it in the div with id="map-canvas".
        It sets the center and zoom of the new map to the
        mapOptions object literal defined above*/ 
        var map = new google.maps.Map(document.getElementById("map-canvas"),
            mapOptions);
      }

      /*using the event addDomListenever to call the 
      initialize method. this is similar to the
      addEventListener method.  we tell the 
      addDomListener which DOM element to use
      (window in this case), what event to listen 
      for (load), and which method to call after 
      the event has occurred.*/

      /*For more info on the DOM check out this video 
      series on the DOM, https://www.youtube.
      com/playlist?list=PL18600E7CA651B16B and this 
      Mozilla article on event listeners, https://developer.
      mozilla.org/en-US/docs/Web/API/EventTarget.
      addEventListener*/
      
      google.maps.event.addDomListener(window, 'load', initialize);

      /*finally to really rip through what is going on here
      and what other things you can do such as asynchronously
      loading the API visit the Google maps getting started 
      doc, https://developers.google.
      com/maps/documentation/javascript/tutorial*/

    </script>
  </head>
  <body>
    <!-- creates a div for the map canvas to be injected into on the html page -->
    <div id="map-canvas"/> 
  </body>
</html>
```
##Dropping a Pen the Map 

``` coffeescript Coffeescript Tricks start:51 mark:52,54-55
# Given an alphabet:
alphabet = 'abcdefghijklmnopqrstuvwxyz'

# Iterate over part of the alphabet:
console.log letter for letter in alphabet[4..8]
```

