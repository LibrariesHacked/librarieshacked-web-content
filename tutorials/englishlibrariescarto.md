---
Title: english public libraries web mapping
Description: using the libraries taskforce dataset to start creating visualisations
Date: 
Type: Tutorial
Author: dave
GitHub: 
TutorialComplexity: medium
TutorialSkillsRequired: GIS,SQL
TutorialNeeded: web browser,Carto
TutorialTime: 1 hour
TutorialType: Maps
Template: tutorialpage
---

On 30th March the Libraries Taskforce released [Public libraries in England: basic dataset](https://data.gov.uk/dataset/public-libraries-in-england-basic-dataset).  This listed all libraries in England (statutory and non-statutory).

- Service (e.g. Gloucestershire)
- Name (e.g. Ashby-De-La-Zouch)
- Address
- Postcode
- Email
- Website

Not necessarily as much as could have been hoped, but a dataset nonetheless.  One limiting factor in the data is there is no good location data for the libraries.  For each property in the UK Ordnance Survey hold detailed address, location coordinates, and a reference - UPRN.  This data is not open data.  It is available as part of an OS product called AddressBase, which is free to use for any public sector organisation, but expensive for others.  However the Ordnance Survey have a presumption to public policy for open data, so with the right requests made (perhaps by the taskforce) it would 

This tutorial will attempt to map those libraries using freely available open data.  This is surprisingly difficult.  Those familiar with mapping tools, perhaps Google fusion tables or MapData, may find this difficult to believe.  Just upload the CSV file into those products and it gives you a map.  That is true, but is it open data?

| Google Fusio Tables |
| MapData |





Step 1. Signup or login to Carto
--------------------------------

Carto (previously known as CartoDB) is a free online map mapping service, though the free tier of membership restricts how much data you can store and doesn't allow private data.

It works by a process of uploading data into it's web interface.  As long as this data then has some kind of spatial component (such as co-ordinates, or even addresses) you can visualise the data in map form, and optionally embed that map into websites.

The [signup](https://carto.com/signup) is quite simple, and does not ask for too many details.

Step 2.  Upload first dataset
-----------------------------

In the Carto dashboard change 'Your maps' to 'Your datasets'

Select to 'Add Dataset'.  This will bring up a form that provides many options for importing data - Upload, Google Drive, Dropbox.  If the data is available online you can simply enter a URL here.  The Libraries Taskforce dataset is at:

https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/604815/Public_libraries_in_England_-_basic_dataset__as_on_1_July_2016_.csv

After entering this and clicking Submit, Carto is then particularly clever and offers to sync the data regularly.  In this case, it's doubtful that this particular dataset will get updated but if it was (and the URL stayed the same), you could ensure that your copy is kept up to date.

There is an important and slightly hidden option which is always checked by default: *'Let CARTO automatically guess data types and content on import'*.  It is worth leaving this checked, as it ensures that data is handled in the right way (such as numbers being treated as numbers, dates being treated as dates, etc).  However,
it can on occasion get confused by certain data and, like Excel, do things like remove leading zeros from data.  So if you do experience trouble you can uncheck this option and each column will be imported as simple text.

In this case you can leave it checked and click, which presents this error.



Oh dear.  Malformed CSV?  CSV (comma separated values) files are a text format for storing data that uses commas to separate out columns.  It is customary, though not mandatory, for the first row to contain the column headers, for example:

```
firstname,surname
dave,bloggs
cath,jones
```

The Taskforce dataset looks like this.

```
Static Public Libraries (open at 1 July 2016),,Location,,,
Library service,Library name,Postal address,Postcode,Contact email,URL of library website
Barking and Dagenham,Barking,"2 Town Square, Barking",IG11 7NB,barking.library@lbbd.gov.uk,www.lbbd.gov.uk/libraries
```

On the face of it, it looks OK.  Downloading it and trying to upload into Carto doesn't work but after converting and saving it as UTF-8 encoding rather than ANSI, it then seems to work fine.  This can be done in a tool like notepad  The lesson here is that data processes rarely go to plan!  Be prepared to do some messing about with data.

It's also worth removing the very first line before uploading as there are two header rows.

Then repeating the process but selecting to upload a file instead of providing a URL it all gets loaded into Carto.  This is effectively creating a new database table in Carto. Carto automatically adds columns for numeric ID and 'the_geom'.



'the_geom is the column that will store the location (geometry) of each library.  There is no recognised location field in the uploaded data so Carto will currently show these as 'null'.

Geocoding the data
------------------

Carto offers some geocoding facilities 'out of the box' which would allow for geocoding from address data (such as ).  In the UK

Also, it's more fun doing it with open data.


Step 3.  Upload Code Point Open
-------------------------------

Code Point Open is an Ordnance Survey Open Data product.  It provides a list of all valid postcode in 

The data comes in a zip file with a file for each postcode area (e.g. BA.csv).  We just want to upload everything in one go so need to combine the files into one.  This can be done using a classic command prompt in windows (cmd.exe).


copy *.csv postcodes.csv


In Linux this could be done


The data is then a little large, and on the free version of Carto we're 

To save time, the associated GitHub repository for this tutorial includes a copy of the final postcodes.csv file that can be uploaded straight to Carto.

Upload that CSV into Carto.



Step 4.  Data checking
----------------------

So with libraries and official postcodes uploaded some data checking can be done.


select * from libraries where replace(postcode, ' ', '') not in (select replace(postcode, ' ', '') from postcodes)

The replace() function around each postcode just removes the spaces when checking for matching postcodes.  This just allows for any situations where the spacing may be different between the postcodes in the libraries and code point open dataset.











