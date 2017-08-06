---
Title: where to find public libraries open data
Description: searching through central gov portals, and local datastores
Type: Article
Tags: open data,local government datastores,public libraries data
Template: blogpage
Author: dave
Date: 2015/12/04
---

Hunting out library open data - in particular for public libraries - can be difficult. Even where a library authority is actively publishing open data (already then making them a minority), the data could appear in multiple different places.

#### data.gov.uk

A quick search for 'library' or 'libraries' on [data.gov.uk](https://data.gov.uk/) will return a large number of results. But filtering through that to find current and open datasets (the **Open Government Licence** is a good filter to use), reveals far less.

Quite a number of these are library location and services data. For examples, Calderdale have an [information on libraries dataset](https://data.gov.uk/dataset/libraries8), released under the Open Government Licence, which looks fabulous in term of the detail published for each library. The fields included are:

<pre class="prettyprint"><code>Library name,Email,Telephone,Fax,Address,Town,Postcode,Disabled Go link,Asian language books,Audiobooks,Access for All,W Yorks Archive Service,Busy Babies,Career guides,Class visits,Foreign language books,Fax facilities,History and photography collection,Internet access,Readers group,Language courses,Significant local history collection,Magazines,Meeting room hire,Newspapers,Open Learning,Prospectuses,Photocopy facilities,Photo-Me,Significant Reference & Information collection,Sales,Quick reference section,Self-issue machine,Study area,Storytimes,Wheelchair Access,Childrens Lending,Request Service,Feature Film DVD's,Sound and Vision,Monday opening times,Tuesday opening times,Wednesday opening times,Thursday opening times,Friday opening times,Saturday opening times,Sunday opening times</code></pre>

One the other hand, North Yorkshire publish [a libraries dataset](https://data.gov.uk/dataset/libraries2) but with the following fields:

<pre class="prettyprint"><code>Library Name,Name,Address 1,Address 2,Address 3,Address 4,Address 5,Postcode,Web Address,Telephone Number,Telephone Number,Email,Longitude,Latitude,Hours open per week,The Building is owned or Leased,Disability Discrimination Act compliant,Visits,Issues - Books and Media,Active borrowers,Active Users,PC Sessions,wi-fi hours used,Home Library and Information,Under 16,16-24,25-54,Over 55,Building costs (£),Other costs (£),Income (£)</code></pre>

In that case, far less information about services, but it does additionally include geo-coordinates and usage data. If anyone was trying to aggregate or collect these datasets to use as a single set, they would end up with only a small subset of common fields - making the rest of the data a waste.

It is also not what fields are published but the schema (or lack of), and standards followed for essentially the same data. Calderdale publish opening hours on a column-per-day basis (e.g. *Sunday opening times*), while North Yorkshire publish an *Hours open per week* total. But there are already [standard schemas](https://schema.org/openingHours) for opening hours that would make interoperability a lot easier. And then there is the mix of technical skills required to access the different datasets - [Warwickshire](https://data.gov.uk/dataset/warwickshire-libraries) publish a good dataset but only in XML form; in general the set of different formats appearing are KML, WMS Feed, CSV, XLS, XML, RDF...

#### LGA data portal

The LG inform portal located at [http://datasets.opendata.esd.org.uk/](http://datasets.opendata.esd.org.uk/) encourages quality open data (e.g. that which could be defined as 5 on [the 5 star rating](http://5stardata.info/en/)). This is because the portal is built to support schemas, where every dataset can publish against a specified schema. It includes a [validator](http://validator.opendata.esd.org.uk/) to validate a particular dataset against a schema, and an [aggregator](http://aggregator.opendata.esd.org.uk/) for retrieving all data released under a particular schema, regardless of publisher. For example all library authorities could release data and it could then be automatically aggregated.

The problem is, it's not well used. Searching for libraries only reveals Leeds, North Somerset, and Redbridge. The [North Somerset dataset](http://datasets.opendata.esd.org.uk/details?datasetId=22003) is a good example of library locations data published to a set schema but with few fields.

<pre class="prettyprint"><code>Library Name,Address,Locality,Postcode,Telephone No.,Website</code></pre>

#### Local government/community web portals

Many local authorities are building data portals to host their own data. [Leeds Data Mill](http://leedsdatamill.org/), [Hampshire Hub](http://www.hampshirehub.net/), [Bath: Hacked](https://data.bathhacked.org/) are all examples of local data portals, and they all include library data of different kinds.

For example, this [previous post on Leeds Data Mill](https://www.librarieshacked.org/tutorials/highcharts) data looks at the usage data published for library loans, and library computer usage. In that case, the datasets are also published on data.gov.uk (as a reference to the Leeds Data Mill location). But such referencing on different portals is rarely the case, meaning each local data portal has to be searched through to get a comprehensive list of open data.

Local data portals are also less likely to use standard schemas - they are good for local data analysis and hacking (creating *smart cities*), but trying to combine them against other local sets is unlikely to be possible in current form.

#### Library websites

Searching for open data on library websites (e.g. either the online catalogue or the authority site) rarely turns up open data - neither type of site are built to support open data releases. There will be reports and stats released:  for example the [Libraries West about page](https://www.librarieswest.org.uk/about/) gives stats for the latest financial year ('Around 7.2 million items were loaned from our libraries last year'), which is interesting to read, but less so in the context of looking for open data. There is also a PDF archive of annual reports which is full of similarly interesting data, with some infographics.

#### Summary

In general, the current set of public library open data serve only to highlight problems with central and local government open data portals, as well as highlighting how little data is available. Even going through all of data.gov.uk, LG Inform, and local data portals there are still only a handful of data publishers and little chance of any aggregation of datasets.