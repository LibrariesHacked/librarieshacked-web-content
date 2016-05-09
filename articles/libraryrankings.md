---
Title: world library rankings
Description: ranking literacy by using library provision
Tags: library rankings, OCLC global stats, literacy
Type: Article
Template: blogpage
Date:
Author: @librarieshacked
---

## OCLC global library stats

A Libraries Hacked tutorial post from 2014 [YQL and global library stats](https://www.librarieshacked.org/tutorials/yqlstats), used the OCLC global library statistics web pages as an example of extracting web data using Yahoo Query Language (YQL).

That was to demo YQL rather than draw attention to the stats themselves.  The [OCLC global library stats](https://www.oclc.org/global-library-statistics.en.html) were an attempt by the OCLC to collect together some global stats for public/academic/school/special libraries such as number of libraries, volumes, members, and librarians.  The data was manually collected and the relevant sources listed for each entry.  

The OCLC appreciate counting libraries across countries is difficult.  They settle on the following method:

<blockquote><p>For the sake of uniformity, the total number of libraries represents administrative units and not service points, since not all sources report service points consistently. Some of the country entries did not specify which unit they were reporting, in which case it was assumed that it was administrative units.</p><small>OCLC - Global Library Statistics.</small></blockquote>

The administrative units figure is a strange one as it often doesn't really represent number of libraries.  Each country will have considerably different systems of administration.  In the UK the administrative unit is generally the local authority, which can significantly vary in size and number of libraries.  Administrative unit is more representative of government, community, geography and spread of population.  It also seems odd to assume that an ambiguous figure given would be administrative units.

Looking through the data and exploring some of the sources it's possible to spot some oddities:

- The UK is declared to have 184 public libraries.  Even if another 1000 public libraries were to close, that number of authorities could potentially stay the same - it gives little indication of library provision.
- Estonia is an example where the number of administrative units seems similar to the complete number of libraries. The source is [from the IFLA](http://db.ifla-world-report.org/home/map#/2/2/G8TWX1EQKSOGSCH): *'Estimated number of public libraries in the country: 560: 487 main libraries, 73 service points'.*  The OCLC list the 487 figure.
- Latvia is listed as having .  This appears to be the total number of branches - from a report on [Economic value and impact of public libraries in Latvia](http://www.kis.gov.lv/download/Economic%20value%20and%20impact%20of%20public%20libraries%20in%20Latvia.pdf)
- The data sources go back quite far.  Morocco is listed as having 5 school libraries, but the date for the measure is given as 1982, and the source a book: **Librarian's companion (2nd ed. Greenwood 1996)**

An excellent site, Practical Statistics, looks at the accuracy of the data, paying particular attention to the data for Norway:

[Practical Statistics - OCLC Initiative](https://sites.google.com/site/practicalstatistics/2-events/ifla-singapore/oclc-initiative)

<blockquote><p>The results are not encouraging. The library statistics that OCLC offered for Norway could not be used for serious work. The quality was far too uneven. OCLC faced the same problem that Unesco faced. Countries with weak library systems have hardly any statistics at all. There may be some numbers floating around, but they do not reflect the situation on the ground. Countries with stronger systems have better statistics, but they do not publish them in a form that is convenient for outsiders.</p><small>Practical Statistics - OCLC Initiative</small></blockquote>

## world literacy rankings

A recent report on **literature behaviour characteristics** ranked Finland as most literate nation, with the UK ranked 17th of 60.

[Guardian - Finland ranked World's most literate nation](http://www.theguardian.com/books/2016/mar/11/finland-ranked-worlds-most-literate-nation)

The report is detailed at [World's Most Literate Nations](http://www.ccsu.edu/wmln/).  It doesn't use only educational test results, but also uses cultural measures: 

- educational investment
- newspaper circulation
- access to computers
- number of public/academic/school libraries, and number of volumes in public libraries

The data used for libraries [turns out to be](http://www.ccsu.edu/wmln/dataSources.html) the stats from the OCLC.  The method used to rank the countries is to take those 4 measures from the OCLC data, adjust for population size and create a rank for that country.  A key missing measure is number of librarians, which doesn't appear to be considered so important.

The data sources and methodology used is very open.  The specific ranking for libraries are listed:

- [Library rankings](http://www.ccsu.edu/wmln/library.html)

Looking through the rankings it's possible to see some patterns:

- The list becomes reflective of population size.  China, with a population of over a billion, are at the bottom of the ranking.  For example, if Estonia have **487** libraries with a population of 1.3 million, China (population 1.3 billion) would need to have around **487,000** to match Estonia in that ranking .  The OCLC stats say it has **3073**.   But is that a fair measure?  Commonly in library consultations distance to nearest library is used as a measure of accessibility.  Size could be used, but really that would need to take into account the particular geography (land use) of each country.
- Where the number of libraries used is not administrative units those countries do better.
- Mistakes in the data get highlighted in the results e.g. Morocco are ranked second from bottom.