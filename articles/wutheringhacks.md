---
Title: wuthering hacks
Description: newcastle libraries open data process and library hack day 
Tags: newcastle libraries, library hacks, open data
Type: Article
Template: blogpage
Date: 2016/05/11
Author: @librarieshacked
---

Newcastle Libraries held a hack day on 9th April - **Wuthering Hacks**.

The event and motivations are blogged about in [Releasing and re-using library data](https://librariestaskforce.blog.gov.uk/2016/04/12/releasing-and-re-using-library-data/), by Aude Charillon on the Libraries Taskforce blog.

## The data

The event was preceded with an unprecedented amount of public libraries open data being released.  This is still listed and available on the Newcastle Council open data site.

- [Newcastle Open Data - Libraries data sets](https://www.newcastle.gov.uk/your-council-and-democracy/open-data-and-access-information/open-data/data-sets/libraries-data-sets)

It includes such datasets as:

- Libraries and their locations (geo-coordinates)
- Monthly computer usage
- Monthly enquiries
- Monthly issues
- Stock - Catalogue/Items
- Consumption in library buildings (electricity/gas/water)

A key to dedicating to an open data process is maintaining it, and not expecting immediate results.  One-off datasets and extracts are useful for a weekend event, but not as a long-term process.  In the datasets above they are all listed with an update frequency, a named maintainer of the data, when they were created, and when they were last updated.

If there is one thing that Newcastle proved even before the event, it's that libraries open data can be done, and there isn't a good reason not to.  Newcastle Council seem fairly new to open data (though doing it very well), and it is refreshing to see the library service making themselves a part of that process, getting involved with the developer community, and putting a comprehensive process in place for their data.

## The day

There were some fascinating projects from the day:

- A project to scan a book using the barcode and automatically play an audio description of that book.  This made use of Google Books as a data source, a mobile device with camera and barcode recognition, and a web speech API. 

- Superimposing of historic maps.  Newcastle libraries also released a set of maps as part of the event.  There is a process for georeferencing these and then merging them using map tools to see how they compare to current day sources (such as Google maps).

- Statistical analysis of usage trends using the [R project](https://www.r-project.org/) - free statistical analysis and graphing software.  It was good to see usage data being engaged with directly for statistical analysis.  Currently libraries usage data such as CIPFA, or Taking Part data isn't easily available to interested statisticians to work with before it has been turned into reports.  With greater access to data in raw formats it allows for use of tools (such as R) that are freely available.

Newcastle Libraries have made it clear this is the beginning of a process of releasing open data and running events.  Although it may be early days, it's currently the best example of a UK public library service having an open data policy and generating use of that data.