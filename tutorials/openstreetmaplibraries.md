---
Title: open street map and libraries
Description: using open street map to find a list of libraries
Date: 
Type: Tutorial
Author: dave
GitHub:
TutorialComplexity: low
TutorialSkillsRequired: GIS
TutorialNeeded: Web broswer
TutorialTime: 1 hour
TutorialType: Maps
Template: tutorialpage
---

This is the first in a series of posts on finding data listings of UK libraries.  These will not necessarily be open data sources (which would be ideal), but any licensing restrictions will be covered.  The first one is Open Street Map.

What is open street map?
------------------------

[Open Street Map](https://www.openstreetmap.org/) (OSM) is *'is a map of the world, created by people like you and free to use under an open licence'*.

As they say, OSM data is open data.  It is released under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/).  As it is community driven, and created by a variety of people, users of OSM data are required to include the credit: **&copy; OpenStreetMap contributors**.

OSM and libraries
-----------------

OSM data is tagged with 

| Wiki | Summary |
| ---- | ------- |
| [Tag: amenity]() |  |
| [Tag: amenity = library](http://wiki.openstreetmap.org/wiki/Tag:amenity%3Dlibrary) |  |

How to get hold of this data though.  Well, it's open data so in theory someone could just download the lot of it and extract the data.  However the [weekly OSM planet XML file](http://planet.openstreetmap.org/) is 56GB, and that's compressed.  Handling that data would be quite an effort.

Xapi
----

[Xapi](https://wiki.openstreetmap.org/wiki/XAPI), the OSM extended API and *'provides enhanced search and querying capabilities'*.

http://api.openstreetmap.fr/xapi

Accessing that URL on its own would just return an error.  It needs a query.

Step 1. Construct the query for libraries
-----------------------------------------

Xapi allows for querying 3 entities in OSM: Nodes, Ways, and Relationships.

| Entity | Description |
| ------ | ----------- |
| Node |  |
| Way |  |
| Relationship |  |


```
http://api.openstreetmap.fr/xapi?node[amenity=library][bbox=-6.37988,49.871159,1.76896,55.811741]
```

Step 2. Run the query
---------------------

The easiest way to get the data is just to request the URL in a browser.  Alternatively, depending on your operating system

### windows

```

```

### linux and mac

```

```

The returned data will be in XML format.  

## task 2: convert the data from XML




```
osmconvert osm-libraries.osm --all-to-nodes --csv="@id @lon @lat amenity name address place building addr:housename addr:street addr:city addr:postcode barrier highway is_in man_made phone wheelchair wheelchair:description operator wifi opening_hours postal_code access internet_access website url wikipedia" --out-csv -o=libraries.csv
```

