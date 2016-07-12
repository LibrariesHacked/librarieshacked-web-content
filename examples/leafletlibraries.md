---
Title: leaflet libraries map
Description: a leaflet map example using culture grid library location data to create a map of uk libraries.
Template: leafletexample
Type: ExampleVisualisation
---

this is an example of using the [leaflet JavaScript library](http://leafletjs.com/) for creating a map, alongside the [marker cluster plugin](https://github.com/Leaflet/Leaflet.markercluster), using uk libraries as points plotted on the map.

leaflet allows developers to create maps with *simplicity, performance and usability* in mind.  it is immensely easy to use, beautiful, and performs far better than a lot of other options (even google maps).  the example below for example has around four thousand markers, but leaflet is designed to cope with tens of thousands. 

marker cluster is a plugin that clusters points on the map based on the distance between them, and the zoom level of the map - in order to conveniently group together points.  clicking on a cluster zooms and expands the points to show further detail.  hovering over a cluster also shows the boundaries of all the points within that group.