---
Title: english public libraries web mapping
Description: using the libraries taskforce dataset to create visualisations
Type: Tutorial
Tags: 
Template: tutorialpage
Date: 2017/08/25
Author: dave
GitHub: 
TutorialType: Maps
TutorialComplexity: medium
TutorialSkillsRequired: GIS,SQL
TutorialNeeded: web browser,carto account
TutorialTime: 1 hour
---

<iframe width="100%" height="520" frameborder="0" src="https://carpentryglassbox.carto.com/builder/add1a40e-f982-4036-9f5d-97ba8b6fb793/embed" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe>

<small>Contains OS data © Crown copyright and database right (2017). Contains Royal Mail data © Royal Mail copyright and Database right 2017. Contains National Statistics data © Crown copyright and database right 2017. Library data released by the Libraries Taskforce under the [Open Government Licence](http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).</small>

On 30th March 2017 the Libraries Taskforce released [Public Libraries in England: Basic Dataset](https://data.gov.uk/dataset/public-libraries-in-england-basic-dataset). This listed all public libraries in England with the following fields:

- Library Service
- Library Name
- Address
- Postcode
- Email
- Website

The data was collected from library authorities in mid-2016.  It includes all types of public libraries: real ones, volunteer, neighbourhood collections, etc.

One limiting factor was location data. For each property in the UK, Ordnance Survey (OS) hold an address, co-ordinates, and a Unique Property Reference Number (UPRN). This data is not open data, though aspects of it can be made open (such as UPRN) within derived datasets. It is available as part of a premium product called [AddressBase](https://www.ordnancesurvey.co.uk/business-and-government/products/addressbase-premium.html), which is free for public sector organisations, but costly for others.

The OS have a [presumption to publish](https://www.ordnancesurvey.co.uk/business-and-government/help-and-support/public-sector/guidance/presumption-to-publish-criteria.html) policy for open data. For public asset datasets, that do not 'substantially copy' their licensed data, it can be presumed these can be open data. In the case of around 3000 libraries, it is unlikely that releasing the locations would affect the commercial viability of AddressBase, or substantially copy it. It would be great if any future library dataset could publish the 'official' co-ordinates, property references, and addresses of library locations, as well as obtaining an exemption from the OS to allow this.

This tutorial will map those libraries using the current dataset and freely available open data. This can be surprisingly difficult. Those familiar with online mapping tools (like Google), will know you normally just upload the address spreadsheet, it magically geocodes it all and shows the map. That's true (ish), but there are normally licence restrictions. Google will geocode addresses for you, but you MUST plot the points on a Google map and use their services, and you cannot then make the co-ordinates open data.

#### Step 1. Signup or login to Carto

Carto (previously known as CartoDB) is an online map mapping service. The free tier of membership restricts how much data you can store and doesn't allow private data.

It works by uploading data into it's web interface. As long as this data has some kind of geography component (such as co-ordinates) you can view maps, and optionally embed those maps into websites.

- In your browser navigate to [https://carto.com/](https://carto.com/).
- In the Menu navigate to **Signup** or go straight to [https://carto.com/signup/](https://carto.com/signup/).
- Fill in the brief details and create a new account.
- When prompted for additional details, fill these out if you wish to.  There's no need to provide a phone number or payment details.

#### Step 2. Upload libraries dataset

- Once logged in, on the Menu navigate to **Maps > Your Datasets**.
- Select **New Dataset**.
- In the URL field enter: **<span class="word-wrap: break-word;">https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/604815/Public_libraries_in_England_-_basic_dataset__as_on_1_July_2016_.csv</span>**
- Click **Submit**
- There is an important and slightly hidden checkbox which is always checked by default: **'Let CARTO automatically guess data types and content on import'**. It's worth initially leaving this checked, as it helps ensure that data is handled in the right way (such as numbers being treated as numbers, dates being treated as dates, etc). However, it can get confused by certain data and, like Excel, do silly things like remove leading zeros. So if you do experience trouble, you can uncheck this option and each column will be imported as simple text and not be messed with.
- There are a number of options as to whether you want to Sync the data. These are useful, but only available with a paid account.  Leave the default of **Never** and select **Connect Dataset**.
- The file all gets loaded into Carto. The name of the dataset becomes **public_libraries_in_england_basic_dataset_as_on_1_july_2016_**.
- That's a bit long for a name. By clicking the three dots menu next to the dataset name and selecting **Rename dataset** you can rename it to something like **libraries**

Carto has automatically added columns for 'ID' and 'the_geom'. 'the_geom' is the column that will store the location (geometry) of each library. There is no recognised location data in the uploaded file, so Carto will currently show these as 'null'.

It also looks like the column names aren't correct. That's because the original data looks like this:

| Static Public Libraries (open at 1 July 2016) |      | Location |      |      |      |
| --------------------------------------------- | ---- | -------- | ---- | ---- | ---- |
| Library service | Library name | Postal address | Postcode | Contact email | URL of library website |
| Bolton | Highfield neighbourhood collection (The Orchards) | The box under the table, The corner, The Foyer, Highfield (The Orchards), Highfield Road, Bolton | BL4 0RN | orchards@bolton.gov.uk | a URL |

It seems to have two header rows, the second one being the 'real' column headers, and the first one only having two entries. Carto uses the first one for it's column headers and fills in missing headers with default ones. Correcting this would mean downloading the file from gov.uk, deleting the first header row in a text editor (like notepad) and then uploading it as a dataset manually into Carto. Or just leaving it and not worrying too much. Will leave it as is for now, it'll just mean not referring to meaningful column names later on.

#### Step 3. Upload Code Point Open

[Code Point Open](https://www.ordnancesurvey.co.uk/business-and-government/products/code-point-open.html) is an Ordnance Survey open data product. It provides a list of all current valid postcodes in Great Britain with their co-ordinates. Because a postcode can contain many properties these co-ordinates represent the centre of the postcode. That causes some issues, a bit like when you put a postcode into a SatNav but end up a mile away from your destination, maybe with a river and motorway in the way. So it's not completely accurate. As libraries are pretty important buildings, they often have a postcode all to themselves. For those ones the location data will be very precise, right on top of the building.

Code Point Open data comes in a zip file with a file for each postcode area (e.g. BA.csv). That's a little tedious. We just want to upload everything in one go, so need to combine the files into one.  The data is also a little large, and on the free version of Carto we're limited to 250Mb. That 250Mb also doesn't necessarily relate to the size of the CSV you want to upload. It's about how much space the data occupies within a database structure.  What we'd need to do is delete columns from the CSV that we don't need. As we're also dealing just with England we could filter the postcodes to only include those from England.

To save some time, there's a CSV file on the libraries hacked GitHub account that's already been processed, so you don't need to download code point open or mess around with it.

- Return to the Data menu (the top left large 'dot'), and select to **Add a new dataset**.
- This time use the URL of our pre-processed postcodes file: **https://github.com/LibrariesHacked/library-carpentry/raw/master/data/postcodes_england.csv**
- Click **Submit**, and **Connnect dataset** to import the data as before.  This may take some extra time (such as 10-20 mins), there's over a million postcodes.

Because it's so large it's worth indexing the postcodes dataset, which should improve performance.

- Return to the Datasets menu and go into the **postcodes_england** dataset.  Select at the bottom the option to show **SQL**.
- Remove the current text shown in the SQL editor and replace with:

<pre class="prettyprint lang-sql"><code>CREATE INDEX postcodesindex ON postcodes_england (postcode)</code></pre>

- Click **Apply**.

This tells the underlying database to create an index (called 'postcodesindex') on the 'postcodes_england' table, using the column called 'postcode'.

#### Step 4. Data checking

So with both our library dataset and official postcodes uploaded, some data checking can be done. Yay!

- Go into the **libraries** dataset.  Select at the bottom the option to show **SQL**.  This will display:

<pre class="prettyprint lang-sql"><code>SELECT * FROM libraries</code></pre>

This tells us that the current data view is showing all columns (*) from the libraries dataset. We can change this to a view that only shows the libraries where the library postcode is NOT in the postcodes dataset (it's probably wrong).

- Change the SQL to the following.

<pre class="prettyprint lang-sql"><code>select
    field_2,
    field_4
from libraries
where replace(field_4, ' ', '') not in
    (select replace(postcode, ' ', '') from postcodes_england)
order by field_2</code></pre>

- Click **Apply**.

The replace() function around each postcode just removes the spaces when checking for matches, to allow for situations where the spacing may be different (e.g. 'BA11RG' and 'BA1 1RG'). 'Field_4' is the field that would have been called 'postcode' in the Taskforce dataset if it didn't have the extra header row. 'Field_2' is the library name, which we also use for alphabetical ordering in the query above.

There are 49 rows returned. These are shown below, with the reasons why they're wrong. (Carto doesn't work this out, I had to look em all up on google!)

| Library name | Dataset postcode | Actual postcode | Reason |
| ------------ | ----------------- | --------------- | ------ |
| Adswood and Bridgehall | SK8 8NR | SK3 8NR | Typo |
| Allington Library | ME6 0PR | ME16 0PR | Typo |
| Alresford | S024 9AQ | SO24 9AQ | Typo |
| Appleby | CA16 1QP | CA16 6QP | Typo |
| Arcadia library and leisure centre | M19 3PH | M19 3AF | Postcode no longer in use |
| Bethnal Green library | E2 0HL | E2 0HW | Postcode no longer in use |
| Bingley | BD16 1AW | BD16 1AJ | Postcode no longer in use |
| Boscombe | BH1 1BY | BH5 1BY | Typo |
| Brampton | CA8 8NX | CA8 1NW | Typo |
| Burton Book Drop | LA6 7NA | LA6 1NA | Typo |
| Chelmsford | CM1 1LH | CM1 1QH | Typo |
| Consett | DH8 5AT | DH8 5AP | Postcode no longer in use |
| Corby | NN17 1QJ | NN17 1QG | Typo |
| Crowborough Library| TN6 1DH | TN6 1FE | Postcode no longer in use |
| Daventry | NN1 4GH | NN11 4GH | Typo |
| Diss Library | IP22 3DD | IP22 4DD | Typo |
| Failsworth Library | M35 0FJ | M35 0AE | Postcode no longer in use |
| Farley Community Centre | LU2 5RE | LU1 5RE | Typo |
| Forest Hall | NE12 0LJ | NE12 7JY | Postcode no longer in use |
| Frinton | C013 9DA | CO13 9DA | Typo |
| Garforth library and one stop centre | LS25 1DU | LS25 1EH | Typo |
| Highley | WV16 6GH | WV16 6JG | Typo |
| Holderness Road Customer Service Centre Library | HU9 2AH | HU9 2BN | Postcode no longer in use |
| Keighley | BD21 3SX | BD21 2AT | Typo |
| Littlemore | OX4 5JY | OX4 6JZ | Typo |
| Long Ashton | BS4 9AH | BS41 9AH | Typo |
| Lymm | WA13 5SL | WA13 0QW | Typo |
| Mansfield Woodhouse | NG18 8AH | NG19 8AH | Typo |
| Newark | NG24 1U | NG24 1UW | Typo |
| Newbiggin Hall Library | NE5 4BZ | NE5 4BR | Typo |
| Newington Library | CT12 6NB | CT12 6FA | Postcode no longer in use |
| Newton Aycliffe Library | DL5 5QG | DL5 4EJ | Postcode no longer in use |
| Offerton | SK2 5NX | SK2 5NW | Postcode no longer in use |
| Old Marston | OX3 3PH | OX3 0PH | Typo |
| Park Library | SN3 2LP | SN3 2LZ | Typo |
| Paulton | BS29 7QG | BS39 7QG | Typo |
| Preston | HA9 8PL | HA9 8LX  | Postcode no longer in use |
| Primrose library | NE32 34AU | NE32 4AU | Typo |
| Redbourn | AL3 3JQ | AL3 7BP | Changed |
| Shard End Library | B34 7AG | B34 7AZ | Postcode no longer in use |
| Shiremoor | NE27 OHJ | NE28 9JQ | Typo |
| Skelton Library | TS12 2HN | TS12 2HP | Typo |
| Thornaby Central Library and Customer Service Centre | TS17 9EU | TS17 9EW | Typo |
| Thrapston | NN14 4SJ | NN14 4JJ | Typo |
| Wakefield One | WF1 2DA | WF1 2EB | Typo |
| Wargrave | EG10 8EP | RG10 8EP | Typo |
| Winslow | MK18 3RB | MK18 3DL | Typo |
| Worksop | S89 2BP | S80 2BP | Typo |

There will be other errors in the postcodes but these are just the ones that have resulted in an invalid postcode.

#### Correct the data

We have a load of corrections we can make to the dataset.

- Go into the **libraries** dataset (if not there already). Select at the bottom the option to show **SQL**. Replace the SQL text with:

<pre class="prettyprint lang-sql"><code>update libraries set field_2 = trim(from field_2);
update libraries set field_4 = trim(from field_4);
delete from libraries where field_2 = 'Library name';
update libraries set field_4 = 'SK3 8NR' where field_2 = 'Adswood and Bridgehall' and field_4 = 'SK8 8NR';
update libraries set field_4 = 'ME160PR' where field_2 = 'Allington Library' and field_4 = 'ME6 0PR';
update libraries set field_4 = 'SO24 9AQ' where field_2 = 'Alresford' and field_4 = 'S024 9AQ';
update libraries set field_4 = 'CA16 6QP' where field_2 = 'Appleby Library' and field_4 = 'CA16 1QP';
update libraries set field_4 = 'M19 3AF' where field_2 = 'Arcadia library and leisure centre' and field_4 = 'M19 3PH';
update libraries set field_4 = 'E2 0HW' where field_2 = 'Bethnal Green library' and field_4 = 'E2 0HL';
update libraries set field_4 = 'BD16 1AJ' where field_2 = 'Bingley' and field_4 = 'BD16 1AW';
update libraries set field_4 = 'BH5 1BY' where field_2 = 'Boscombe Library' and field_4 = 'BH1 1BY';
update libraries set field_4 = 'CA8 1NW' where field_2 = 'Brampton Library' and field_4 = 'CA8 8NX';
update libraries set field_4 = 'LA6 1NA' where field_2 = 'Burton Book Drop' and field_4 = 'LA6 7NA';
update libraries set field_4 = 'CM1 1QH' where field_2 = 'Chelmsford library' and field_4 = 'CM1 1LH';
update libraries set field_4 = 'DH8 5AP' where field_2 = 'Consett Library' and field_4 = 'DH8 5AT';
update libraries set field_4 = 'NN17 1QG' where field_2 = 'Corby' and field_4 = 'NN17 1QJ';
update libraries set field_4 = 'TN6 1FE' where field_2 = 'Crowborough Library' and field_4 = 'TN6 1DH';
update libraries set field_4 = 'NN11 4GH' where field_2 = 'Daventry' and field_4 = 'NN1 4GH';
update libraries set field_4 = 'IP22 4DD' where field_2 = 'Diss Library' and field_4 = 'IP22 3DD';
update libraries set field_4 = 'M35 0AE' where field_2 = 'Failsworth Library' and field_4 = 'M35 0FJ';
update libraries set field_4 = 'LU1 5RE' where field_2 = 'Farley Community Centre' and field_4 = 'LU2 5RE';
update libraries set field_4 = 'NE12 7JY' where field_2 = 'Forest Hall' and field_4 = 'NE12 0LJ';
update libraries set field_4 = 'CO13 9DA' where field_2 = 'Frinton library' and field_4 = 'C013 9DA';
update libraries set field_4 = 'LS25 1EH' where field_2 = 'Garforth library and one stop centre' and field_4 = 'LS25 1DU';
update libraries set field_4 = 'WV16 6JG' where field_2 = 'Highley' and field_4 = 'WV16 6GH';
update libraries set field_4 = 'HU9 2BN' where field_2 = 'Holderness Road Customer Service Centre Library' and field_4 = 'HU9 2AH';
update libraries set field_4 = 'BD21 2AT' where field_2 = 'Keighley' and field_4 = 'BD21 3SX';
update libraries set field_4 = 'OX4 6JZ' where field_2 = 'Littlemore' and field_4 = 'OX4 5JY';
update libraries set field_4 = 'BS41 9AH' where field_2 = 'Long Ashton' and field_4 = 'BS4 9AH';
update libraries set field_4 = 'WA13 0QW' where field_2 = 'Lymm' and field_4 = 'WA13 5SL';
update libraries set field_4 = 'NG19 8AH' where field_2 = 'Mansfield Woodhouse' and field_4 = 'NG18 8AH';
update libraries set field_4 = 'NG24 1UW' where field_2 = 'Newark' and field_4 = 'NG24 1U';
update libraries set field_4 = 'NE5 4BR' where field_2 = 'Newbiggin Hall Library' and field_4 = 'NE5 4BZ';
update libraries set field_4 = 'CT12 6FA' where field_2 = 'Newington Library' and field_4 = 'CT12 6NB';
update libraries set field_4 = 'DL5 4EJ' where field_2 = 'Newton Aycliffe Library' and field_4 = 'DL5 5QG';
update libraries set field_4 = 'SK2 5NW' where field_2 = 'Offerton' and field_4 = 'SK2 5NX';
update libraries set field_4 = 'OX3 0PH' where field_2 = 'Old Marston' and field_4 = 'OX3 3PH';
update libraries set field_4 = 'SN3 2LZ' where field_2 = 'Park Library' and field_4 = 'SN3 2LP';
update libraries set field_4 = 'BS39 7QG' where field_2 = 'Paulton (The Hub)' and field_4 = 'BS29 7QG';
update libraries set field_4 = 'HA9 8LX' where field_2 = 'Preston' and field_4 = 'HA9 8PL';
update libraries set field_4 = 'NE32 4AU' where field_2 = 'Primrose library' and field_4 = 'NE32 34AU';
update libraries set field_4 = 'AL3 7BP' where field_2 = 'Redbourn' and field_4 = 'AL3 3JQ';
update libraries set field_4 = 'B34 7AZ' where field_2 = 'Shard End Library' and field_4 = 'B34 7AG';
update libraries set field_4 = 'NE28 9JQ' where field_2 = 'Shiremoor' and field_4 = 'NE27 OHJ';
update libraries set field_4 = 'TS12 2HP' where field_2 = 'Skelton Library' and field_4 = 'TS12 2HN';
update libraries set field_4 = 'TS17 9EW' where field_2 = 'Thornaby Central Library and Customer Service Centre' and field_4 = 'TS17 9EU';
update libraries set field_4 = 'NN14 4JJ' where field_2 = 'Thrapston' and field_4 = 'NN14 4SJ';
update libraries set field_4 = 'WF1 2EB' where field_2 = 'Wakefield One' and field_4 = 'WF1 2DD';
update libraries set field_4 = 'RG10 8EP' where field_2 = 'Wargrave' and field_4 = 'EG10 8EP';
update libraries set field_4 = 'MK18 3DL' where field_2 = 'Winslow' and field_4 = 'MK18 3RB';
update libraries set field_4 = 'S80 2BP' where field_2 = 'Worksop' and field_4 = 'S89 2BP';
update libraries set field_4 = 'TA24 8NP' where field_2 = 'Porlock' and field_4 = 'TA24 7HD';</code></pre>

- Click **Apply**.

Whew! Quite a few updates.

#### Step 5. Create a new dataset

We can then create a new dataset with the location data from the postcodes table, and the library data from the libraries table.

- Go into the **libraries** dataset (if not there already). Select at the bottom the option to show **SQL**. Replace the SQL text with:

<pre class="prettyprint lang-sql"><code>select
    l.static_public_libraries_open_at_1_july_2016,
    l.field_2,
    l.location,
    l.field_4,
    l.field_5,
    l.field_6,
    p.the_geom
from libraries l
join postcodes_england p
on replace(p.postcode, ' ', '') = replace(l.field_4, ' ', '')</code></pre>

- Click **Apply**. This will create a new view of the data.
- On the three dots menu next to the dataset title click **Create Dataset from query** (the first option).

This goes away and creates a new dataset, which will be called 'libraries_copy'. Rename this (**Rename dataset** on the same three dot menu) to something more descriptive like 'librariesgeo'.

#### Step 6: Make a map

- When in the **librariesgeo** dataset click the **Create Map** button in the bottom right hand corner.

This creates a new map and takes you into the map editor tool. From here on there is lots you can do to customise the map. When you're done the map can be published with the **Share** button.  An example is embedded at the top of this page.