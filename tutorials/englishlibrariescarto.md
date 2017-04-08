---
Title: english public libraries web mapping
Description: Using the Libraries Taskforce dataset to start creating visualisations
Date: 2017/04/06
Type: Tutorial
Author: @librarieshacked
GitHub: 
TutorialComplexity: medium
TutorialSkillsRequired: GIS,SQL
TutorialNeeded: web browser, Carto
TutorialTime: 1 hour
TutorialType: Maps
Template: tutorialpage
---

On 30th March the Libraries Taskforce

## 1.  Signup or login to Carto

Carto (previously known as CartoDB) is a free online map mapping service, though the free tier of membership restricts how much data you can store, and doesn't allow private data, unless you use a paid subscription.

It works by uploading data into it's web interface.  As long as this data then has some kind of spatial component (such as co-ordinates, or even addresses) you can visualise these on a map, and embed that map into websites, or as standalone link.

The [signup](https://carto.com/signup) is quite simple, and does not ask for too many details.

## 2.  Upload first dataset.

In the Carto dashboard change 'Your maps' to 'Your datasets'


Select to 'Add Dataset'.  This will bring up a form that provides many options for importing data - Upload, Google Drive, Dropbox.  If the data is available online you can simply enter a URL here.  The Libraries Taskforce dataset is at:

https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/604815/Public_libraries_in_England_-_basic_dataset__as_on_1_July_2016_.csv

After entering this and clicking Submit, Carto is then particularly clever and offers to sync the data regularly.  In this case, it's doubtful that this particular dataset will get updated but if it was (and the URL stayed the same), you could ensure that your copy is kept up to date.

There is an important and slightly hidden option which is always checked by default: *'Let CARTO automatically guess data types and content on import'*.  It is worth leaving this checked, as it ensures that data is handled in the right way (such as numbers being treated as numbers, dates being treated as dates, etc).  However,
it can on occasion get confused by certain data and, like Excel, do things like remove leading zeros from data.  So if you do experience trouble you can uncheck this option and each column will be imported as simple text.

In this case you can leave it checked and click, which presents this error.



Oh dear.  Malformed CSV?  CSV (comma separated values) files are a text format for storing data that uses commas to separate out columns.  It is customary, though not manadatory, for the first row to contain the column headers, for example:

```
firstname,surname
dave,bloggs
cath,jones
```

The Taskforce dataset looks this this.

```
Static Public Libraries (open at 1 July 2016),,Location,,,
Library service,Library name,Postal address,Postcode,Contact email,URL of library website
Barking and Dagenham,Barking,"2 Town Square, Barking",IG11 7NB,barking.library@lbbd.gov.uk,www.lbbd.gov.uk/libraries 
...more rows of data
```

On the face of it, it looks OK.  Downloading it and trying to upload into Carto doesn't work but after saving it as UTF-8 encoding rather than ANSI, it then seems to work fine.  The lesson here is that data processes rarely go to plan!  Be prepared to do some messing about with data.

It's also worth removing the very first line as there are two header rows.

Then repeating the process but selecting to upload a file instead of providing a URL it all gets loaded into Carto.  Effectively this is then creating a single database table.  Carto automatically adds a numeric ID and a 'the_geom' column:

'the_geom is the column that will store the location (geometry) of each library.  
