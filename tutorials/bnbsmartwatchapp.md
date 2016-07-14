---
Title: a british national bibliography smartwatch app
Description: creating a smartwatch app to show books published and set in the current location.
Date: 2015/07/30
Type: Tutorial
Author: @librarieshacked
GitHub: Pebble-BNBBooks
TutorialType: WebDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: JavaScript,SPARQL,PebbleJS
TutorialNeeded: web browser
TutorialTime: 1 hour
Template: tutorialpage
---

With the Apple watch, various Android watches, and Microsoft [releasing the Band](https://www.microsoft.com/microsoft-band/en-gb), 2015 may still be set to be the year of the smart watch (though this telegraph article suggests in terms of **wearables** it is the [year of the smart bra](http://www.telegraph.co.uk/news/predictions/technology/11306735/wearable-technology-trend.html)).

## pebble
Pebble is a Kickstarter funded smart watch, the second generation version holds the record for highest funded project at around $2 million.  It is also relatively cheap (the first one around £80), integrates with iOS and Android phones, and includes an easy cloud based-development environment, [CloudPebble](http://cloudpebble.net).

Although it is yet to be seen how successful the second watch will be, or how popular smart watches will turn out to be in general, Pebble is at least a fun learning tool for writing simple watch apps that make use of the features on the phone paired with the watch.

## pebble.js
Like hybrid apps on phones, pebble has created an option for developers to write apps (**watchapps**) purely using a JavaScript library, **Pebble.JS**.  A [basic guide](http://developer.getpebble.com/guides/js-apps/pebble-js/) to Pebble.JS on the Pebble site shows how to set up an app.

## example app - finding books published nearby
As an example, this app will:

- query the [British National Bibliography](http://bnb.bl.uk/) (BNB) for a book that was published in the user's current location.
- also query for a book **set** in the current location.

## setting up the app
The app can be written within the cloud pebble environment, which includes a simulator for testing.  All the code is written within a single app.js file.  CloudPebble also allows easy integration with GitHub, so the full code for this app is located at:

[https://github.com/LibrariesHacked/Pebble-BNBBooks](https://github.com/LibrariesHacked/Pebble-BNBBooks)

After following steps in the [guide to starting cloud pebble development](http://developer.getpebble.com/guides/js-apps/pebble-js/), the initial set-up of the app is to design the screen, which just consists of a single UI 'card' with title, subtitle, and body text.

<pre class="prettyprint linenums">
<code>var ui = require('ui');
var ajax = require('ajax');
// Test location for use in simulator
var testLocation = { coords: { latitude: 51.94, longitude: -2.26 } };

// Create and display the main UI card - values can then be dynamically changed
var main = new ui.Card({
  title: 'BNB Books',
  subtitle: 'Search for local books',
  body: 'Push buttons!'
});
main.show();</code>
</pre>

## handling events

The code needs to perform actions when each button is pressed.  These are available as JavaScript **events**, so event handlers are written in the same way as traditional web-based JavaScript (such as clicking a button on a webpage).

The Pebble watch has three buttons.  Top, middle and bottom.  Using the top and bottom for each process, in the code below, each action triggers a call to get the current location (**navigator.geolocation.getCurrentPosition**).

The Pebble watch itself does not does not have an Internet connection, or the ability to detect location - all these features are obtained via a Bluetooth connection to a phone.  The JavaScript library hides that though, and handles the communication between the phone and the watch.

<pre class="prettyprint linenums">
<code>var locationOptions = { "timeout": 15000, "maximumAge": 60000 };
// Variable used for tracking the current function (which button was pressed)
var currentFunction = '';
// Up button (top) will find books published in the current location (BNB)
main.on('click', 'up', function(e) {
  currentFunction = 'published';
  navigator.geolocation.getCurrentPosition(reverseGeocode, handleError, locationOptions);
});
// Down button (bottom) finds books set in the current location (BNB)
main.on('click', 'down', function(e) { 
  currentFunction = 'setin';
  navigator.geolocation.getCurrentPosition(reverseGeocode, handleError, locationOptions);
});</code>
</pre>

## getting location

The **navigator.geolocation.getCurrentPosition** method takes in three arguments.  These are:

- a method to run when the location has been retrieved;
- a method to run if there is an error;
- some options for the request to get location (such as a timeout value).

In the example above, once the location is retrieved, both actions go on to run a method called reverseGeocode.

## what is reverse geocoding?

Geocoding is getting location coordinates (typically in latitude and longitude) from descriptive data (such as an address).  In the case of location data from GPS, we start off with the location coordinates and want to turn that into something descriptive (e.g. Tewkesbury) - reverse geocoding.

Open Street Map can be used for reverse geocoding.  A single call to a URL (**http://nominatim.openstreetmap.org/reverse?**), passing in the latitude and longitude will return an address, which can then be used for querying the BNB.

<pre class="prettyprint linenums">
<code>function reverseGeocode(pos) {
  pos = testLocation;
  var lat = pos.coords.latitude;
  var lon = pos.coords.longitude;
  var url = 'http://nominatim.openstreetmap.org/reverse?format=json&addressdetails=1&lat=' + lat + '&lon=' + lon;
  ajax( { url: url, type: 'json' },
       function(data, status, request) {
         if (currentFunction == 'published') getBookPublished(data.address);
         if (currentFunction == 'setin') getBookSetIn(data.address);
       },
       handleError
      );
}</code>
</pre>

## querying the BNB

The BNB data has an open SPARQL endpoint, allowing remote querying.  It also has a [test editor online](http://bnb.data.bl.uk/flint-sparql) for trying out queries.

It is worth reading [Leigh Dodds' 4 part series](http://blog.ldodds.com/2014/10/08/an-introduction-to-the-british-national-bibliography/) introduction to the BNB, as well as looking through the GitHub repository here for a great [set of sample SPARQL queries](https://github.com/ldodds/bnb-queries).

Rather than showing the JavaScript code to pass through SPARQL queries, these are two example queries that would be used to find books set and published in Gloucester.

### books published in a location

<pre class="prettyprint linenums">
<code>PREFIX bibo: &lt;http://purl.org/ontology/bibo/&gt;
PREFIX blt: &lt;http://www.bl.uk/schemas/bibliographic/blterms#&gt;
PREFIX dct: &lt;http://purl.org/dc/terms/&gt;
PREFIX event: &lt;http://purl.org/NET/c4dm/event.owl#&gt;
PREFIX rdfs: &lt;http://www.w3.org/2000/01/rdf-schema#&gt;
SELECT ?book ?title ?isbn ?timeLabel ?creator ?name WHERE {
   ?place rdfs:label "Gloucester" .
   ?publication event:place ?place.
   ?book
      blt:publication ?publication;
      bibo:isbn10 ?isbn;
      dct:creator ?creator;
      dct:title ?title.
}</code>
</pre>

### books set in a location

A notable point about getting books set in a location is that location is most are typically formatted like 'Gloucester (England)'.  So some manipulation of the address data has to be done to combine city and state.

<pre class="prettyprint linenums">
<code>PREFIX bibo: &lt;http://purl.org/ontology/bibo/&gt;
PREFIX blt: &lt;http://www.bl.uk/schemas/bibliographic/blterms#&gt;
PREFIX dct: &lt;http://purl.org/dc/terms/&gt;
PREFIX event: &lt;http://purl.org/NET/c4dm/event.owl#&gt;
PREFIX foaf: &lt;http://xmlns.com/foaf/0.1/&gt;
PREFIX rdf: &lt;http://www.w3.org/1999/02/22-rdf-syntax-ns#&gt;
PREFIX rdfs: &lt;http://www.w3.org/2000/01/rdf-schema#&gt;
PREFIX c4dm: &lt;http://purl.org/NET/c4dm/event.owl#&gt;
SELECT ?book ?title ?isbn ?creator ?name WHERE {
   ?setIn rdfs:label "Gloucester (England)" .
   ?publication event:place ?place;
      c4dm:time ?time.
   ?book
      a bibo:Book;
      blt:bnb ?bnb;blt:publication ?publication;
            bibo:isbn10 ?isbn;
            dct:title ?title;
            dct:creator ?creator;
            dct:spatial ?setIn.
         ?creator foaf:name ?name.
}</code>
</pre>

## display the data

The easiest way to display the data within a Pebble.JS app is just to update the home card values (title, subtitle, body text) with whatever data has been found.

<pre class="prettyprint linenums">
<code>function displayItem(title, subtitle, body){
  main.title(title);
  main.subtitle(subtitle);
  main.body(body);
}</code>
</pre>

And that's it.