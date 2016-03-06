/*
Title: IP data - mapping applicant postcodes
Description: Looking at intellectual property trademark and patent location data to produce interactive maps.
Tags: mapping, intellectual property, patents
Type: Article
Template: blogpage
Date: 2016/03/04
Author: @librarieshacked
*/

*This is the first of two posts focussing on Intellectual Property (IP) data in preparation for an IP Magnet hackathon being held in Manchester Library on 12th March 2016.  Rather than being a tutorial post, this is a description of the datasets and processes involved in mapping the currently available IP datasets..*
[EventBrite - IP Magnet Challenge](https://www.eventbrite.co.uk/e/the-ip-magnet-challenge-intellectual-property-data-hackathon-tickets-21674235221)

Intellectual property (IP) applications (patents, trademarks) are handled in the UK by the Intellectual Property Office (IPO).  This data has for a long time been public (as it needs to be), and searchable through such tools as the IP patents and trademarks journals:

- [Intellectual Property Office - Searchable Patents Journal](https://www.ipo.gov.uk/p-pj)
- [Intellectual Property Office - Trademarks Journal](https://www.ipo.gov.uk/t-tmj/)

There are also various developer/consumer services for access to this data.  Weekly updates to the patent journal are published in XML or PDF format:

- [Patents journal download](https://www.ipo.gov.uk/types/patent/p-os/p-journal/p-pj-download.htm) PDF or Basic XML

For statistical research, these services aren't so useful - web pages are useful for searching for specific information, rather than data analysis. Weekly XML would also have to be compiled together in order to be turned into a current snapshot of the full data, and would ideally be in a more accessible format to non-developers. 

There are a couple of datasets listed on Gov.uk, where the data has been *compiled into one snapshot in order for statistical research to be undertaken*.  A full description of the fields is also available on the relevant pages for each dataset.

- [Gov.UK IPO: Patent data](https://www.gov.uk/government/publications/ipo-patent-data) (updated 26th October 2015) A snapshot of patent applications received and subsequently published by the Intellectual Property Office.
- [Gov.UK IPO: Trade mark data](https://www.gov.uk/government/publications/ipo-trade-mark-data-release) (23rd February 2015)

Licence: The data is released under an Open Government Licence

## Patents data

The snapshot data for patents includes postcode districts for each application - for the applicant(s), and the current proprietor(s).

<table class="table">
<tr><td>Column</td><td>Description</td></tr>
<tr><td>Applicant/s at application</td><td>The name of the applicant/s first recorded for the given application. Applicants separated by ;</td></tr>
<tr><td>Postcode/s of applicant at application</td><td>Postcode/s of the applicant/s at application (note this only contains the area code and present for UK applicants). Postcodes are separated by ;</td></tr>
<tr><td>Current proprietor/s</td><td>The name of the current proprietor/s of this patent. The proprietor could be the same as the applicant but will differ if the applicant has changed name or ownership of the patent application has changed hands. Proprietors are separated by ;</td></tr>
<tr><td>Current proprietor/s postcode/s</td><td>The postcode/s of the current proprietor/s. Postcodes are separated by ;</td></tr>
</table>

There are 52,3367 records.  Of these only 15,9913 actually have postcode information, representing the number of applications from the UK.

## Analysing location data

Comparing the two columns for applicant and current proprietor column, it appears that there are only 34,239 records where they are different, around a fifth of the total.

As there is not much difference, taking just the applicant postcode district data, this post will look at visualising the amounts of applications coming from different postcode districts.  For geographic visualisations, choropleth maps (from the Latin, simply *coloured maps*) can be useful to visualise differences in areas. This is colouring the relevant areas based upon some kind of value for that area (in this case number of applications).

## Postcode districts

Postcode data is split into different levels of accuracy.

<table class="table">
<tr><th>Level</th><th>Example</th><th>Description</th></tr>
<tr><td>Postcode area</td><td>M</td><td>The area is identified by one or two alpha characters at the start of the full postcode, the letters being derived from a town, city or district falling within the postcode area.  There are, at present, 120 postcode areas in Great Britain.</td></tr>
<tr><td>Postcode district</td><td>M2</td><td>A sub-area of the postcode area, specified by the character sub-string within the first half of a full postcode.  There are approximately 2 800 postcode districts in Great Britain.</td></tr>
<tr><td>Postcode sector</td><td>M2 5</td><td>A sub-area of a postcode district, whose area is identified by the number third from the end of a full postcode. There are approximately 9000 postcode sectors in Great Britain.</td></tr>
<tr><td>Postcode unit</td><td>M2 5PD</td><td></td>An area covered by a particular postcode. Postcodes are an alphanumeric abbreviated form of address. Postcode units are unique references and identify an average of 15 addresses.  There are approximately 1.7 million postcode units in the UK.</tr>
</table>

(Data abbreviated from Ordnance Survey [Linked Open Data Ontologies](http://data.ordnancesurvey.co.uk/ontology/postcode/PostcodeDistrict).)

## Postcode open data

The Ordnance Survey have a free open data product called Code-Point open which provides coordinate-based location information for all 1.8M postcode units.

- [Code-point Open](https://www.ordnancesurvey.co.uk/business-and-government/products/code-point-open.html)

However, what is often useful for geographic analysis (and essential for choropleth maps) is to have polygon data - areas that define the boundaries around particular postcode districts/areas/sectors.

This data is not freely available from the OS though, as it is included in the OS premium product:

- [Code-point with polygons](https://www.ordnancesurvey.co.uk/business-and-government/products/code-point-with-polygons.html)

*(It is worth nothing that public sector organisations should have free access to OS premium products as part of the [Public Sector Mapping Agreement](https://www.ordnancesurvey.co.uk/business-and-government/public-sector/mapping-agreements/public-sector-mapping-agreement.html)).*

Instead of using OS polygions, the boundaries can be derived from the point data.  If you were to plot all of the postcodes in the UK on a map and then draw a line around all the 'sets' (whether that set is area/district/srea/unit) then that set of shapes is effectively the polygon data.

Luckily this has already been done from OS open data, by a Geospatial organisation called Geolytix.

- [Geolytix Open Data](http://geolytix.co.uk/geodata/).

Licence: Released under the same terms as the OS OpenData license with the single addition of GeoLytix to the attribution list.

## CartoDB

Having done the job of hunting out the relevant data sets, [CartoDB](https://cartodb.com/) is an online tool *to map and analyse location data*.  A free account (restricted by enforcing that datasets must be public, and 250Mb limit) is generally enough to do a lot of visualisation work.

Both the Geolytix open data, and the Gov.UK snapshots of IPO data can be directly uploaded into CartoDB as datasets that can then be joined (using the postcode district in both sets) and manipulated using structured query langugage (SQL), within CartoDB.  The [CartoDB docs](http://docs.cartodb.com/) themselves are the best places to get started if you want to delve into mapping data within the tool.

## Filtering data to Manchester

Given that the IP Magnet challenge is being held in Manchester it makes sense to focus on that area.  Filtering data can also aid in display as any kind of interactive visualisation is going to perform better with a smaller set of data.

Wikipedia here lists the districts within the M postcode area:

- [Wikipedia: M Postcode area](https://en.wikipedia.org/wiki/M_postcode_area)

The map below, produced using CartoDB shows the relevant postcode districts in the M postcode area, coloured to show the number of IP data applicants.  Clicking on a particular area will show some more detailed information.

<iframe width="100%" height="520" frameborder="0" src="https://daveroweuk.cartodb.com/viz/a0bf12e8-e2e4-11e5-8559-0e787de82d45/embed_map" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe>