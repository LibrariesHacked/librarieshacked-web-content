---
Title: public libraries and indices of deprivation
Description: analysing the 2015 indices of deprivation data with relation to lower layer super output areas and the location of libraries
Date: 2015/10/01
Type: Tutorial
TutorialType: Maps
TutorialComplexity: medium
TutorialSkillsRequired: GIS, SQL
TutorialNeeded: web browser, Carto
TutorialTime: 1 hour
Template: tutorialpage
---

On Wednesday, 30th September the UK government released the updated [2015 English indices of deprivation](https://www.gov.uk/government/statistics/english-indices-of-deprivation-2015). 

For **32,844** defined areas in England, a deprivation rank is given (1 being the most deprived, 32844 being the least), in 7 different category areas.  These are:

- Income
- Employment
- Education, skills and training
- Health deprivation and disability
- Crime
- Barriers to housing and services
- Living environment

A **decile** is then calculated by ordering those 32,844 areas into ten groups and assigning each a rating of 1 to 10 (with 1 being within the most deprived and 10 being within the least).  The 7 categories also go on to make an overall *multiple index of deprivation*, giving a single 1-10 index per area.

In data about English public libraries, the index of deprivation is notably also included in the [DCMS Taking Part survey data](https://www.gov.uk/government/statistics/taking-part-201516-quarter-2-statistical-release), with the respondents being fairly evenly distributed across the 10 different deprivation indices.

The geographical areas used are **Lower Layer Super Output Areas** (LSOAs), defined by the Office for National Statistics to create a set of comparable areas based on number of people (between 1,000 and 3,000) and households (400 - 1,200).  The data can be downloaded as spatial data, with the areas provided as polygons (providing the boundary lines):

[2011 Lower Layer Super Output Area (LSOA) boundaries](http://www.ons.gov.uk/ons/guide-method/geography/products/census/spatial/2011/index.html)

The indices, and associated LSOAs, offer a way of comparing areas across the country, and can be combined with other datasets that use the same defined areas (such as population estimates, age of population, religion, etc).  When assessing council services, you would expect very little assessment to be done in a particular area without reference to those LSOAs and their respective deprivation indices.

## libraries in deprived areas

When assessing library services, an important consideration may be the accessibility of the library for areas that are most deprived (and by association, those with most need).  But outside of individual authority lists, there is no comprehensive list of libraries, or in particular which LSOAs they are located in.

Taking an older, though still relatively recent dataset, the list of libraries on data.gov.uk (published last in 2012) included locations of libraries:

[Public libraries contacts](https://data.gov.uk/dataset/uk-public-library-contacts-14032012)

With that data, along with the defined LSOA areas, and the deprivation indices for those LSOAs, it is possible to do some analysis on which areas public libraries tend to be located, in terms of deprivation.

## Carto

[Carto](https://Carto.com/) is a paid-for mapping and data service that offers a free option for a relatively small number of datasets (maximum 250Mb), which generally covers personal use.  The service is built on PostGIS, an open source database specialised for GIS (geographic information systems) querying.  The analysis could all be done directly on a PostGIS database, but Carto has a convenient web interface and also includes many flashy map visualisations such as chloropleth maps (coloured/shaded areas).

Three sets of data can be uploaded into Carto:  the 2015 indices of deprivation, the LSOA polygon data, and the libraries point data.

1. When the LSOA areas are uploaded Carto will recognise the GIS data and also include the LSOA codes as a lookup against the geographic polygons.
2. The indices are then uploaded which also include the LSOA codes.
3. The library points data is then loaded.

Those datasets being in Carto effectively become three database tables that can be queried and joined.  Due to it being built using PostGIS, those queries include the full power of spatial SQL.

For example, a SQL query can be written that counts the number of libraries per multiple deprivation index decile:

<pre class="prettyprint linenums lang-sql">
<code>SELECT 
id.IMDDecile,
SUM((SELECT count(*) FROM libraries li WHERE ST_Intersects(li.the_geom, ls.the_geom))) AS libcount
FROM lsoas2011 ls
JOIN indicesdeprivation id
ON ls.lsoa11cd = id.lsoa_code_2011
GROUP BY id.IMDDecile</code>
</pre>

The spatial part of the query above is the **ST_Intersects** fuction, which is calculating whether a library falls within the bounds of an LSOA.  Even though there is no direct link in the library data to LSOAs, if the location is known then the spatial query can make that link.  The three tables in the above example are named **libraries** (the library locations), **indicesdeprivation** (the 2015 indices), and **lsoas2011** (the LSOA boundaries).  By grouping by the index of multiple deprivation decile (*IMDDecile*), a sum of the total libraries for each decile can be returned.

| Index of multiple deprivation decile | Library count |
| ------------------------------------ | ------------- |
| 1 | 424 |
| 2 | 367 |
| 3 | 354 |
| 4 | 324 |
| 5 | 300 |
| 6 | 319 |
| 7 | 296 |
| 8 | 255 |
| 9 | 311 |
| 10 | 248 |

Or, looking at the data another way, which LSOAs have the most libraries within them (and the associated multiple index of deprivation)?

<pre class="prettyprint linenums lang-sql">
<code>SELECT
ls.lsoa11nm,
id.IMDDecile,
(SELECT COUNT(*) FROM Libraries li WHERE ST_Intersects(li.the_geom, ls.the_geom)) AS libcount
FROM LSOAs2011 ls
JOIN IndicesDeprivation id
ON ls.lsoa11cd = id.lsoa_code_2011
ORDER BY libcount DESC
LIMIT 3</code>
</pre>

| LSOA Name | Index of multiple deprivation decile | Library count |
| --------- | ------------------------------------ | ------------- |
| Richmond upon Thames 008B | 8 | 3 |
| Westminster 023E | 4 | 3 |
| City of London 001F | 7 | 3 |

Or find the libraries in the most deprived areas.

<pre class="prettyprint linenums lang-sql">
<code>SELECT
id.IMDRank,
li.name
FROM LSOAs2011 ls
JOIN IndicesDeprivation id
ON ls.lsoa11cd = id.lsoa_code_2011
JOIN libaries li
ON (ST_Intersects(li.the_geom, ls.the_geom))
WHERE l.name IS NOT NULL
ORDER BY id.IMDRank ASC
LIMIT 4</code>
</pre>

| Index of multiple deprivation rank | Library name |
| ---------------------------------- | ------------ |
| 3 | Blackpool Central Library |
| 33 | Wheatsheaf Library |
| 34 | Freedom Centre Library |
| 34 | Marfleet Library |

And by replacing **ASC** with **DESC** (to use descending order rather than ascending), this gives the ones in the least deprived areas:

| Index of multiple deprivation rank | Library name |
| ---------------------------------- | ------------ |
| 32843 | Wargrave Library |
| 32837 | Binfield Library |
| 32819 | Long Crendon Community Library |
| 32803 | Berkhamsted Library |

Though only being a few years old the library data soon begins to show inaccuracies and limitations - quite a few hundred of those libraries will now be closed.  Having a comprehensive national set of library locations is essential not just for obvious uses, such as tracking closures, but for real data analysis, and understanding how libraries have an impact on deprivation in their immediate area, by combining with other data sources.