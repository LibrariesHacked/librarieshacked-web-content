---
Title: english public libraries web mapping
Description: using the libraries taskforce dataset to create visualisations
Type: Tutorial
Tags: 
Template: tutorialpage
Date: 
Author: dave
GitHub: 
TutorialType: Maps
TutorialComplexity: medium
TutorialSkillsRequired: GIS,SQL
TutorialNeeded: web browser,carto account
TutorialTime: 1 hour
---

On 30th March 2017 the Libraries Taskforce released [Public libraries in England: Basic Dataset](https://data.gov.uk/dataset/public-libraries-in-england-basic-dataset).  This listed all libraries in England (statutory and non-statutory).  It included:

- Library Service (e.g. Gloucestershire)
- Library Name (e.g. Ashby-De-La-Zouch)
- Address
- Postcode
- Email
- Website

The data was collected from library authorities in mid 2016.

One limiting factor in the data is there isn't good location data for the libraries. For each property in the UK, Ordnance Survey (OS) hold an address, exact location co-ordinates, and a reference: Unique Property Reference Number (UPRN). This data is not open data, though aspects of it can be (such as UPRN) within derived datasets. It is available as part of an premium OS product called [AddressBase](https://www.ordnancesurvey.co.uk/business-and-government/products/addressbase-premium.html), which is free to use by any public sector organisation, but costly for others.

The OS have a [presumption to publish](https://www.ordnancesurvey.co.uk/business-and-government/help-and-support/public-sector/guidance/presumption-to-publish-criteria.html) policy for open data. For datasets regarding public assets, that do not 'substantially copy' their licensed data, it can be presumed these can be open data. In the case of around 3000 libraries, it is unlikely that making those open data would affect the commercial viability of the AddressBase product, or substantially copy it. So, it would be useful for any future library dataset to publish the exact coordinates, and addresses of library locations, as well as obtaining an explicit exemption from the OS to allow for this.

This tutorial will attempt to map those libraries using the current dataset and freely available open data. This can be surprisingly difficult. Those familiar with online mapping tools (like Google or MapData), will know you normally just upload the CSV with addresses in, it geocodes it all and shows the points. That's true, but there are normally licence restrictions. Google will geocode addresses for you, but you then HAVE to plot the points on a Google map and use their services, and you cannot then make the coordinate data open.

#### Step 1. Signup or login to Carto

Carto (previously known as CartoDB) is a free online map mapping service, though the free tier of membership restricts how much data you can store and doesn't allow private data.

It works by a process of uploading data into it's web interface.  As long as this data then has some kind of geography component (such as co-ordinates) you can visualise the data in map form, and optionally embed that map into websites.

The [signup](https://carto.com/signup) is quite simple, and does not ask for too many details.

#### Step 2.  Upload first dataset

In the Carto dashboard change 'Your maps' to 'Your datasets'

Select to 'Add Dataset'.  This will bring up a form that provides many options for importing data - Upload, Google Drive, Dropbox.  If the data is available online you can enter a URL here. The Libraries Taskforce dataset is at:

https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/604815/Public_libraries_in_England_-_basic_dataset__as_on_1_July_2016_.csv

There is an important and slightly hidden checkbox which is always checked by default: *'Let CARTO automatically guess data types and content on import'*. It is worth initially leaving this checked, as it often ensures that data is handled in the right way (such as numbers being treated as numbers, dates being treated as dates, etc).  However, it can on occasion get confused by certain data and, like Excel, do silly things like remove leading zeros from text fields as it thinks they're numbers. So if you do experience trouble you can uncheck this option and each column will be imported as simple text and remain unmodified.

In this case you can leave it checked and click 'Submit', which presents this error.



Oh dear.  Malformed CSV?  CSV (comma separated values) files are a text format for storing data that uses commas to separate out columns.  It is normal, though not mandatory, for the first row to contain the column headers, for example:

```
firstname,surname
dave,bloggs
cath,jones
```

The Taskforce dataset looks like this:

```
Static Public Libraries (open at 1 July 2016),,Location,,,
Library service,Library name,Postal address,Postcode,Contact email,URL of library website
Barking and Dagenham,Barking,"2 Town Square, Barking",IG11 7NB,barking.library@lbbd.gov.uk,www.lbbd.gov.uk/libraries
```

On the face of it, it looks OK. Downloading it and trying to upload into Carto also doesn't work, but after saving it (using Notepad++) as UTF-8 encoding rather than ANSI, it then seems to work fine. The lesson here perhaps is that data manipulation rarely goes to plan! Be prepared to do some messing about with data.

It's also worth removing the very first line before uploading as there are two header rows.

Selecting to upload a file instead of providing a URL, the corrected file all gets loaded into Carto.  Behind the scene, this is creating a new database table in Carto. Carto also automatically adds columns for 'ID' and 'the_geom'.

'the_geom is the column that will store the location (geometry) of each library. There is no recognised location data in the uploaded file so Carto will currently show these as 'null'.

#### Geocoding the data

Carto offers some geocoding facilities which would allow for geocoding from address data (such as an address).  In the UK

However, as previously mentioned, 

Also, it's more fun doing it with open data.


Step 3. Upload Code Point Open
-------------------------------

Code Point Open is an Ordnance Survey Open Data product.  It provides a list of all valid postcodes in 

The data comes in a zip file with a file for each postcode area (e.g. BA.csv). That's a little tedious. We just want to upload everything in one go, so need to combine the files into one. This can be done using a classic command prompt in windows (cmd.exe).


copy *.csv postcodes.csv


The data is then a little large, and on the free version of Carto we're limited to 250Mb.  That 250Mb also doesn't necessarily relate to the size of the CSV you want to upload. It's about how much space the data occupies within a database structure.

What we need to do then is delete columns from the CSV that we don't need.  Code Point Open includes

To save time, the associated GitHub repository for this tutorial includes a copy of the final postcodes.csv file that can be uploaded straight to Carto.

Upload that CSV into Carto.



#### Step 4.  Data checking

So with both our library dataset and official postcodes uploaded, some data checking can be done.

select * from libraries where replace(postcode, ' ', '') not in (select replace(postcode, ' ', '') from postcodes)

The replace() function around each postcode just removes the spaces when checking for matching postcodes.  This just allows for any situations where the spacing may be different between the postcodes in the libraries and code point open dataset.


| Library name | Original postcode | Actual postcode | Reason |
| ------------ | ----------------- | --------------- | ------ |
| Paulton | BS29 7QG | BS39 7QG | Typo |
| Shard End Library | B34 7AG | ? | Mystery. Postcode is no longer in use but still listed for the library |
| Boscombe | BH1 1BY | BH5 1BY | Typo |
| Bingley | BD16 1AW | ?
| Keighley | BD21 3SX | BD21 2AT | Incorrect on library website.
| Preston Community | HA9 8PL | ? |
| Winslow Community| MK18 3RB | MK18 3DL |  |
| Appleby | CA16 1QP | CA16 6QP | 1 should have been a 6. |
| Brampton | CA8 8NX | CA8 1NW | 
| Burton Book Drop | LA6 7NA | LA6 1NA | Incorrect on library website.  Postcode for Burton Memorial Hall is LA6 1NA. |
| Consett | DH8 5AT | ? | Postcode no longer exists.
| Newton Aycliffe | DL5 5QG | DL5 4EH | Postcode no longer exists.  Library relocated to Newton Aycliffe Leisure Centre. |
| Crowborough | TN6 1DH | ? | Postcode no longer exists.  Incorrect on library website.  
| Chelmsford | CM1 1LH | CM1 1QH | Typo.  L should have been a Q. |
| Frinton | C013 9DA | CO13 9DA | Typo. 0 should have been O. |
| Alresford | S024 9AQ | SO24 9AQ | Typo. 0 should have been O. |

There will be other errors in the postcodes but these are just the ones that have resulted in an invalid postcode.



