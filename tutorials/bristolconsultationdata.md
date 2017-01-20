---
Title: data analysis during a library consultation
Description: delving into the data used to close libraries.
Date: 2015/04/19
Type: Tutorial
Author: @librarieshacked
GitHub: 
TutorialComplexity: medium
TutorialSkillsRequired: mapping,JavaScript,web services
TutorialNeeded: a text editor
TutorialTime: 2 hours
TutorialType: WebDevelopment
Template: tutorialpagebristollibraries
---

Bristol have entered a period of consultancy [with a proposal](
https://www.bristol.gov.uk/committee/2015/ua/ua000/0303_5.pdf) that aims to (amongst other things) close 7 of the 28 libraries in the authority.  Whilst any actions appear to be temporarily postponed ([plans to close seven Bristol libraries on hold](http://www.bbc.co.uk/news/uk-england-bristol-31909805)), there are no guarantees those libraries will stay open.

Along with survey data from a preliminary 'Bristol library futures' public consultation, the proposal draws from a number of data sources, including some geographical (GIS) data.

## GIS data
Locations of the libraries and the Bristol authority boundary lines are used to visualise library coverage: the libraries are plotted with circular radius of 1.5 miles to show the plans still give majority coverage across the authority.  Using a radius as a measure comes with various issues:

- For non-walking access it is largely irrelevant.  Travel to a nearest library needs to be measured in journey time, accessibility and expenditure.
- For walking, the radius is effectively 'as the crow flies' and can be very different to the journey time.  At best it is a minimum possible distance assuming a perfectly straight pedestrianised route.  To calculate real walking distances would require more in depth route analysis.  Councils have very good GIS teams and software tools (e.g. they may need to calculate the routes that waste collection teams will take across the whole authority); it is not unfeasible to expect an analysis of distance to libraries based on real route data.

The 1.5 mile figure is given as one that is better than a 'recommended minimum' of 2 miles:

<blockquote>
Good geographical access across the city with all residents being within 1.5 miles* of a library.<br/><br/>
*2 mile access was the recommended minimum distance advised by the Secretary of State response to Bolton MBC following a local inquiry – CMS 231060/DC 31 May 2013
<small>bristol libraries for the future proposals for consultation.</small>
</blockquote>

It's hard to track down this *'recommended minimum'* (or maximum?).  A letter from Vaizey to Bolton Council, [with the same reference number](https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/204269/Morris__final_.pdf) reveals a reference to 2 miles but within the context of Bolton infrastructure:

<blockquote>
96% of the population will be within 2 miles of a library, and that almost the entire borough is within 20 minutes journey of a library by public transport, plus walking time to the nearest bus stop. Car ownership is also high in Bolton and other transport services are provided for the elderly and disabled. The Secretary of State views this as sufficient...
<small>Ed Vaizey to Councillor Morris</small>
</blockquote>

In this case the distance was assessed with the following qualifying factors (all of which are specific to the local area):

- Car ownership (in neither Bolton nor Bristol can car ownership be said to be *high* - [2011 census data](http://www.racfoundation.org/assets/rac_foundation/content/downloadables/car%20ownership%20rates%20by%20local%20authority%20-%20december%202012.pdf) shows them to be ranked 269th and 275th out of 348 authorities).
- Public transport links
- Support for the elderly and disabled

## visualising the map data
The basic map within the PDF provided by Bristol council provides a visualisation of coverage, but doesn't allow residents to see (for example) what a 1 mile radius for each library would look like, or explore in detail which areas get the highest coverage - or to see the effect on coverage that closing 7 of the libraries would have. 

To independently visualise these variations on the map data, a couple of datasets need to be used:

- The Bristol authority and ward boundaries.  Available on the Bristol open data portal: [https://opendata.bristol.gov.uk/Government/WD-2011-BFE-South-West/fgti-bng9](https://opendata.bristol.gov.uk/Government/WD-2011-BFE-South-West/fgti-bng9)
- The locations of Bristol libraries: available by accessing layer data from the Bristol council site. [my neighbourhood page](http://www.bristol.gov.uk/page/my-neighbourhood) and the [libraries data layer](http://maps.bristol.gov.uk/arcgis/rest/services/ext/localinfo/MapServer/61/query?where=SDO_NN%28SHAPE%2C%20MDSYS.SDO_GEOMETRY%282001%2CNULL%2CMDSYS.SDO_POINT_TYPE%28353602%2C176781%2C0%29%2CNULL%2CNULL%29%2C%20%27sdo_batch_size%3D0%27%29%20%3D%20%27TRUE%27%20AND%20ROWNUM%20%3C%3D%2010&returnGeometry=true&outFields=*&f=json)

With those two layers, the data can be plotted on a web based leaflet map with some fairly short HTML and JavaScript.  A couple of HTML buttons and functions provide the means to change the radius based on pre-defined values (in this case 1 mile, 1.5 miles, and 2 miles).

The associated map is shown below, with the code sample underneath.

<div class="btn-toolbar">
  <div class="btn-group">
    <a class="btn btn-primary" href="javascript:showProposedLibraries()">proposed libraries</a>
    <a class="btn btn-primary" href="javascript:showAllLibraries()">all libraries</a>
  </div>
  <div class="btn-group">
    <a class="btn btn-success" href="javascript:changeRadius(1609)">1 mile radius</a>
    <a class="btn btn-success" href="javascript:changeRadius(2413.5)">1.5 mile radius</a>
    <a class="btn btn-success" href="javascript:changeRadius(3218)">2 mile radius</a>
  </div>
</div>
<br/>
<div id="map"></div>

## plugins used
The HTML below uses a number of JavaScript scripts.  These are:

- Leaflet maps: to provide the mapping engine
- Bristol libraries: a file containing the Bristol library geolocations
- Bristol boundaries: a file to give the boundary info.
- Proj4JS: provides an easy way to convert from the northing/easting values usually used by local authorities, and the latitude/longitude values preferred by online mapping engines.
- MapFunctions: JavaScript to generate the map, code included below the HTML.

## HTML code
<pre class="prettyprint linenums">
<code>&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.css" /&gt;
    &lt;link href='https://api.tiles.mapbox.com/mapbox.js/plugins/leaflet-label/v0.2.1/leaflet.label.css' rel='stylesheet' /&gt;
    &lt;style&gt;
        #map { width: 100%; height: 500px; border: 1px solid #ccc; }
        .customLabel {color: #ccc !important; border: none; background: none; font-size: 7px; }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class="container"&gt;
        &lt;button onclick="showProposedLibraries()"&gt;show proposed libraries&lt;/button&gt;
        &lt;button onclick="showAllLibraries()"&gt;add closures&lt;/button&gt; | 
        &lt;button onclick="changeRadius(1609)"&gt;1 mile radius&lt;/button&gt;
        &lt;button onclick="changeRadius(2413.5)"&gt;1.5 mile radius&lt;/button&gt;
        &lt;button onclick="changeRadius(3218)"&gt;2 mile radius&lt;/button&gt;
        &lt;div id="map"&gt;&lt;/div&gt;
    &lt;/div&gt;
    &lt;script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"&gt;&lt;/script&gt;
    &lt;script src="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.js"&gt;&lt;/script&gt;
    &lt;script src='https://api.tiles.mapbox.com/mapbox.js/plugins/leaflet-label/v0.2.1/leaflet.label.js'&gt;&lt;/script&gt;
    &lt;script src="mapfunctions.js"&gt;&lt;/script&gt;
    &lt;script src="bristolBoundary.js"&gt;&lt;/script&gt;
    &lt;script src="bristolLibraries.js"&gt;&lt;/script&gt;
    &lt;script src="proj4.js"&gt;&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;</code>
</pre>

## map functions JavaScript
<pre class="prettyprint linenums">
<code>var closingCircles, libraryCircles, map, closingDisplayed = false;

$(document).ready(function () {
    // set up the leaflet map.  initial view of Bristol centre.
    map = L.map('map').setView([51.45, -2.60], 12);
    L.tileLayer('http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}' + (L.Browser.retina ? '@2x' : '') + '.png', {
        attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OSM</a> contributors, <a href="http://cartodb.com/attributions">CartoDB</a>'
    }).addTo(map);

    // add the boundary layer for Bristol.
    L.geoJson(bristolBoundary, {
        weight: 2, fill: true, fillOpacity: 0.7, fillColor: '#FFF', color: '#999999', clickable: false,
        pointToLayer: function (feature, latlng) {
            return L.circleMarker(latlng, { opacity: 0, fillOpacity: 0, clickable: false });
        }
    }).addTo(map);
    // By default use 1609 metres.  
    addLayers(1609);
});

function showProposedLibraries() {
    // remove the libraries suggested for closure
    if (closingDisplayed) map.removeLayer(closingCircles);
    closingDisplayed = false;
}

function showAllLibraries() {
    // add the libraries suggested for closure
    if (!closingDisplayed) map.addLayer(closingCircles);
    closingDisplayed = true;
}

function changeRadius(rad) {
    // remove everything and add again with specified radius
    map.removeLayer(libraryCircles);
    if (closingDisplayed) map.removeLayer(closingCircles);
    addLayers(rad);
}

function addLayers(radius) {

    // adds all the layers from the bristolLibraries data included on the page.
    closingCircles = L.layerGroup();
    libraryCircles = L.layerGroup();

    $.each(bristolLibraries, function () {
        var latlng = NEToLL(this.attributes.X, this.attributes.Y);
        var displayRadiusOptions = { clickable: false, weight: 1, stroke: false, fillColor: '#6699FF' };
        var displayMarkerOptions = { weight: 1, fillColor: '#6699FF' };
        var popupText = '&lt;h4&gt;' + this.value + '&lt;/h4&gt;' + this.attributes.Street + ', ' + this.attributes.Locality + ', ' + this.attributes.Town + ', ' + this.attributes.Postcode;
        if (this.closing) {
            closingCircles.addLayer(L.circle([latlng.latitude, latlng.longitude], radius, displayRadiusOptions));
            closingCircles.addLayer(L.circleMarker([latlng.latitude, latlng.longitude], displayMarkerOptions).setRadius(5).bindPopup(popupText));
        }
        else {
            libraryCircles.addLayer(L.circle([latlng.latitude, latlng.longitude], radius, displayRadiusOptions));
            libraryCircles.addLayer(L.circleMarker([latlng.latitude, latlng.longitude], displayMarkerOptions).setRadius(5).bindPopup(popupText));
        }
    });

    map.addLayer(libraryCircles);
    if (closingDisplayed) map.addLayer(closingCircles);
}

// function to convert between british national grid and lat/lng
// uses proj4js plugin
function NEToLL(east, north) {
    proj4.defs("NationalGrid", "+proj=tmerc +lat_0=49 +lon_0=-2 +k=0.9996012717 +x_0=400000 +y_0=-100000 +ellps=airy +towgs84=446.448,-125.157,542.06,0.15,0.247,0.842,-20.489 +units=m +no_defs");
    var res = proj4('NationalGrid', 'WGS84', [east, north]);
    return { latitude: res[1], longitude: res[0] };
}</code>
</pre>