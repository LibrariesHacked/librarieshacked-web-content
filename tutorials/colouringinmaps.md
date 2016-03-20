---
Title: colouring-in open data maps
Description: using QGIS to create colouring-in maps from OS open data.
Date: 2015/09/18
Type: Tutorial
TutorialType: Maps
TutorialComplexity: medium
TutorialSkillsRequired: GIS, colouring
TutorialNeeded: web browser, QGIS
TutorialTime: 1 hour
Template: tutorialpage
---

Open data can be a headache.  **Ownership** of data isn't straightforward, and if datasets include data under a third-party licence then it can be difficult to separate out what can and can't be published.  Also, data that an organisation may consider themselves to own may be *derived* from third party data.

In local government, the biggest block to open data publishing is OS licensing restrictions.  For example, a local Council will create planning datasets which include *point* and *polygon* data (the points and boundaries on a map that the planning application refers to).  Even though this data will have been created by the local authority, it will have been done so with the aid of licensed data and mapping tools.  So even though those planning applications are public information, and made available for anyone to view, a lot of the most important information about them is derived and cannot be made *open* to use.

For a confusing read, the [Sharing derived data](https://www.ordnancesurvey.co.uk/business-and-government/help-and-support/public-sector/guidance/derived-data-exemptions.html) section of the OS website describes examples of what can and can't be shared.

[Anecdotal evidence](http://www.independent.co.uk/arts-entertainment/art/features/colouring-books-for-adults-how-the-french-are-going-crazy-for-crayolas-9883103.html) suggests that stress and headaches can be treated by colouring books.  Perhaps under the knowledge that their licensing restrictions can be stresful, the OS have released a [series of colouring-in maps](http://www.ordnancesurvey.co.uk/blog/2015/08/maps-join-adult-colouring-in/).

The OS only supply a few cities though, it would be nice to be able to create maps for any area of the UK.  One area in which the OS have released some recent open data, is their [Open Maps Local product](https://www.ordnancesurvey.co.uk/business-and-government/products/os-open-map-local.html), allowing anyone to use certain map layers for any purpose.

## creating an OS colouring map

### download the data

Follow the links through the OS site to [download the open data](https://www.ordnancesurvey.co.uk/opendatadownload/products.html).  It involves selecting which National Grid reference squares you wish to download (e.g. ST), and some brief registration data as the link to download will be sent by email.

The download comes in a zipped up package with the following structure:

- data: the shapefile file data.
- doc: a licence file.
- resources: optional map styling files that can be loaded when editing the map layers.

The data is in ESRI Shapefile data format.   A link to a guide for this data is given at the following location:

[OS Open Map Getting Started](http://www.ordnancesurvey.co.uk/docs/user-guides/os-open-map-local-getting-started-guide.pdf)

There are many GIS tools that will open shapefiles - ESRI's ArcGIS Desktop, Pitney Bowes' MapInfo, but the open source option is [QGIS](http://qgis.org/en/site/), a fully featured and free GIS tool.

### open up in QGIS

The data folder from the OS looks full of files and it can be daunting trying to figure out what needs to be opened.  For example:

- ST_Building.dbf
- ST_Building.prj
- ST_Building.shp
- ST_Building.shx

In the case above, those 4 files are related to a single layer - the *.shp* file being the one to be opened in QGIS.  

On loading *QGIS Desktop*, from the menu select *Add vector layer* either from the left toolbar, or the *Layer > Add Layer* menu.  To locate the source file browse to one of the *.shp* files from the data directory.

To get a decent map it can be enough just to load the following layers:

- Buildings
- Roads
- Woodland
- Surface water

There are then options that may be useful depending on the particular map area such as railways lines, stations, tidal water, roundabouts, etc.

### style the map

Style each individual layer by right clicking the layer name in the layer pane window and selecting *properties*.  From there, style details can be customised such as:

- Line colours, pattern, weight, fill colours
- Labels, fonts
 
After a little tweaking a map suitable for colouring in can be created (no fill colours and suitable spacing between lines).  It can be useful to use differing light pattern fills (e.g. spots, dashed lines, etc) for buildings, woodland, and water.

### print or save as image

To export for printing QGIS has the *Print Composer* tool.  Selecting *Project > New Print Composer* will launch the tool, the current map can then be loaded into the print view by selecting *Layout > Add map*.  This then gives a selection tool to design the map area on the printed page (can be full screen for plain images).

More details on using the print composer tool are given at the QGIS tutorials and tips site:

[Making a map - from part 7](http://www.qgistutorials.com/en/docs/making_a_map.html)

The result (with questionable colours), printed out onto A5 can serve as a colouring exercise over coffee, and may double as a postcard.

<img src="/images/MapColouringInSmall.jpg" class="img-responsive" alt="Colouring in">
