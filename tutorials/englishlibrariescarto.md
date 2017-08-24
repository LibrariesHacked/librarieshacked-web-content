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

On 30th March 2017 the Libraries Taskforce released [Public libraries in England: Basic Dataset](https://data.gov.uk/dataset/public-libraries-in-england-basic-dataset). This listed all public libraries in England with the following fields:

- Library Service (e.g. Bolton)
- Library Name (e.g. Highfield neighbourhood collection (The Orchards))
- Address (e.g. The box, The corner, The Foyer, Highfield (The Orchards), Highfield Road, Bolton)
- Postcode (e.g. BL4 0RN)
- Email (e.g. orchards@bolton.gov.uk)
- Website (e.g.http://www.bolton.gov.uk/website/pages/OrchardsChildrensCentre.aspx - no longer working)

The data was collected from library authorities in mid 2016.  It includes all types of public libraries: real, volunteer, neighbourhood collections, etc.

One limiting factor in the data is there isn't good location data for the libraries. For each property in the UK, Ordnance Survey (OS) hold an address, co-ordinates, and a reference: Unique Property Reference Number (UPRN). This data is not open data, though aspects of it can be (such as UPRN) within derived datasets. It is available as part of an premium OS product called [AddressBase](https://www.ordnancesurvey.co.uk/business-and-government/products/addressbase-premium.html), which is free to use by any public sector organisation, but costly for others.

The OS have a [presumption to publish](https://www.ordnancesurvey.co.uk/business-and-government/help-and-support/public-sector/guidance/presumption-to-publish-criteria.html) policy for open data. For datasets regarding public assets, that do not 'substantially copy' their licensed data, it can be presumed these can be open data. In the case of around 3000 libraries, it is unlikely that making those open data would affect the commercial viability of the AddressBase product, or substantially copy it. So, it would be useful for any future library dataset to publish the 'official' coordinates, property reference, and addresses of library locations, as well as obtaining an explicit exemption from the OS to allow for this.

This tutorial will attempt to map those libraries using the current dataset and freely available open data. This can be surprisingly difficult. Those familiar with online mapping tools (like Google or MapData), will know you normally just upload the CSV with addresses in, it magically geocodes it all and shows the map. That's true (ish), but there are normally licence restrictions. Google will geocode addresses for you, but you then HAVE to plot the points on a Google map and use their services, and you cannot then make the coordinates open data.

#### Step 1. Signup or login to Carto

Carto (previously known as CartoDB) is a free online map mapping service, though the free tier of membership restricts how much data you can store and doesn't allow private data.

It works by a process of uploading data into it's web interface.  As long as this data has some kind of geography component (such as co-ordinates) you can visualise the data in map form, and optionally embed that map into websites.

- In your browser navigate to [https://carto.com/](https://carto.com/).
- In the Menu navigate to **Signup** or navigate to [https://carto.com/signup/](https://carto.com/signup/).
- Fill in the brief details and create a new account.
- When prompted for additional details, fill these out if you wish to.  There's no need to provide a phone number.

#### Step 2.  Upload first dataset

- Once logged in, on the Menu navigate to **Maps > Your Datasets**.
- Select **New Dataset**.
- In the URL field enter: **https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/604815/Public_libraries_in_England_-_basic_dataset__as_on_1_July_2016_.csv**
- Click **'Submit'**
- There is an important and slightly hidden checkbox which is always checked by default: *'Let CARTO automatically guess data types and content on import'*. It is worth initially leaving this checked, as it often ensures that data is handled in the right way (such as numbers being treated as numbers, dates being treated as dates, etc).  However, it can on occasion get confused by certain data and, like Excel, do silly things like remove leading zeros from text fields as it thinks they're numbers. So if you do experience trouble you can uncheck this option and each column will be imported as simple text and remain unmodified.
- There are a number of options as to whether you want to Sync the data. These are useful, but only available with a paid account.  Leave the default of **'Never'** and select **'Connect Dataset'**.
- The file all gets loaded into Carto. The name of the dataset becomes 'public_libraries_in_england_basic_dataset_as_on_1_july_2016_'.
- By clicking the three dots menu next to the dataset name you can rename the dataset to something like 'libraries'

Carto has automatically added columns for 'ID' and 'the_geom'. 'the_geom' is the column that will store the location (geometry) of each library. There is no recognised location data in the uploaded file, so Carto will currently show these as 'null'.

It also looks like the column names aren't correct. That's because the original data looks like this:

| Static Public Libraries (open at 1 July 2016) |      | Location |      |      |      |
| --------------------------------------------- | ---- | -------- | ---- | ---- | ---- |
| Library service | Library name | Postal address | Postcode | Contact email | URL of library website |
| Barking and Dagenham | Barking | "2 Town Square, Barking" | IG11 7NB | barking.library@lbbd.gov.uk | www.lbbd.gov.uk/libraries |

It seems to have two header rows, the second one being the 'real' column titles. Sorting it out means downloading the file from gov.uk, deleting the first header row in a text editor (like notepad) and then uploading it as a dataset manually into Carto. Or just leaving it and not worrying too much. Will leave it as it is, it'll just mean not referring to meaningful column names later on and having a rogue row of data in there that is actually column titles.

Step 3. Upload Code Point Open
-------------------------------

[Code Point Open](https://www.ordnancesurvey.co.uk/business-and-government/products/code-point-open.html) is an Ordnance Survey Open Data product. It provides a list of all valid postcodes in Great Britain.

The data comes in a zip file with a file for each postcode area (e.g. BA.csv). That's a little tedious. We just want to upload everything in one go, so need to combine the files into one. This could be done using a classic command prompt in windows (cmd.exe).

```
copy *.csv postcodes.csv
```

The data is also a little large, and on the free version of Carto we're limited to 250Mb. That 250Mb also doesn't necessarily relate to the size of the CSV you want to upload. It's about how much space the data occupies within a database structure.  What we need to do then is delete columns from the CSV that we don't need. 

To save time, there's a file on the libraries hacked GitHub account that's already been processed so you don't need to download code point open or mess around with it.

- Return to the Data menu (the top left large 'dot'), and then select to **'Add a new dataset'**.
- This time use the URL: **https://github.com/LibrariesHacked/library-carpentry/raw/master/data/postcodes_england.csv**
- Click 'Submit', 'Connnect dataset' to import the data as before.  This may take some extra time (such as 10 mins), there's over a million postcodes.

Because it's so large it's worth indexing the postcodes dataset, which should improve performance.

- Return to the Data menu and go into the **postcodes_england** dataset.  Select at the bottom the option to show **SQL**.
- Remove the current text shown in the SQL editor and replace with:

```SQL
CREATE INDEX postcodesindex ON postcodes_england (postcode)
```

- Click **Apply**.

This tells the underlying database to create an index (called 'postcodesindex') on the 'postcodes_england' table, using the column called 'postcode'.

#### Step 4.  Data checking

So with both our library dataset and official postcodes uploaded, some data checking can be done. Yay!

- Go into the **libraries** dataset.  Select at the bottom the option to show **SQL**.  This will display:

```SQL
SELECT * FROM libraries
```

This tells us that the current data view is showing all columns (*) from the libraries dataset. We can change this to a view that only shows data when the postcode is NOT in the postcodes dataset.

- Change the SQL to the following.

```
select * from libraries where replace(postcode, ' ', '') not in (select replace(postcode, ' ', '') from postcodes)
```

The replace() function around each postcode just removes the spaces when checking for matches to allow for any situations where the spacing may be different between matching postcodes (e.g. BA16AX and BA1 6AX).

| Library name | Original postcode | Actual postcode | Reason |
| ------------ | ----------------- | --------------- | ------ |
| Paulton | BS29 7QG | BS39 7QG | Typo |
| Shard End Library | B34 7AG | ? | Mystery. Postcode is no longer in use but still listed for the library |
| Boscombe | BH1 1BY | BH5 1BY | Typo |
| Bingley | BD16 1AW | ?
| Keighley | BD21 3SX | BD21 2AT | Incorrect on library website.
| Preston Community | HA9 8PL | ? |
| Winslow Community| MK18 3RB | MK18 3DL | ? |
| Appleby | CA16 1QP | CA16 6QP | 1 should have been a 6. |
| Brampton | CA8 8NX | CA8 1NW | ? |
| Burton Book Drop | LA6 7NA | LA6 1NA | Incorrect on library website.  Postcode for Burton Memorial Hall is LA6 1NA. |
| Consett | DH8 5AT | ? | Postcode no longer exists. |
| Newton Aycliffe | DL5 5QG | DL5 4EH | Postcode no longer exists.  Library relocated to Newton Aycliffe Leisure Centre. |
| Crowborough | TN6 1DH | ? | Postcode no longer exists.  Incorrect on library website. |
| Chelmsford | CM1 1LH | CM1 1QH | Typo.  L should have been a Q. |
| Frinton | C013 9DA | CO13 9DA | Typo. 0 should have been O. |
| Alresford | S024 9AQ | SO24 9AQ | Typo. 0 should have been O. |

There will be other errors in the postcodes but these are just the ones that have resulted in an invalid postcode.

#### Correct the data

We have a load of corrections we can make to the dataset.

