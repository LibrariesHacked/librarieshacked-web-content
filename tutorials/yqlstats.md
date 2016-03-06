/*
Title: YQL and global library stats
Description: using YQL to query and merge OCLC global library stats.
Type: Tutorial
Date: 2014/07/11
Template: tutorialpageyqlstats
TutorialType: DataAnalysis
TutorialComplexity: medium
TutorialSkillsRequired: javascript,xml,json,sql
TutorialNeeded: nothing
TutorialTime: 1 hour
*/
## what is YQL?
YQL is a query language developed by yahoo, and used for querying and combining data from around the web; there is more information here at yahoo developer pages, and it's worth exploring for anyone involved in web/app development and web services:

[https://developer.yahoo.com/yql/](https://developer.yahoo.com/yql/)

## what global library stats?
The OCLC attempted a few years ago to produce a large set of stats for *'the total global library universe'*, to provide data on expenditure, users, librarians, volumes, and libraries across the world. the relevant page is here:

[http://oclc.org/global-library-statistics.en.html](http://oclc.org/global-library-statistics.en.html)

The page allows the user to select a particular country and see the stats for that country.  There's no easy way to view the data for different countries side by side, or to sort the data between countries (e.g. country with the most academic librarians, etc.).

Aside from sending an email to the OCLC and asking for a copy of the data, if an individual and wanted to see any graphical comparison between different countries they would normally need to create a spreadsheet and copy and paste the stats they wanted in order to do further analysis.

YQL makes tasks like that more interesting, allowing querying of data sources from web services, and even just querying *webpages* for data (e.g. selecting all the items from within a drop-down list), and merging that with other sources.

Note: the reliability and quality of this particular data is questionable, as covered very well in this paper:

[https://sites.google.com/site/practicalstatistics/2-events/ifla-singapore/oclc-initiative](https://sites.google.com/site/practicalstatistics/2-events/ifla-singapore/oclc-initiative)

There are some particular oddities that may just be mistakes in the data (200 public libraries in the UK?) but regardless, the aim of this is to demonstrate using YQL as a tool to tackle this kind of problem, and not to take on the quality of the data itself (yet).

### step 1: get familiar with the YQL console
YQL itself acts as a web service, allowing users to construct queries that may pull in data from many sources and provide one single call to that data from the user's website, or app.

The YQL console however is a way of working on queries, experimenting, and seeing the results without having to call it from code.  Once the query is refined there is the option to copy the *REST url* that can be used to retrieve the data in future.  The console is at the following address:

[https://developer.yahoo.com/yql/console/](https://developer.yahoo.com/yql/console/)

### step 2: find out where the data is coming from
To be able to query the data from YQL we need to understand where it is coming from.  The easiest way to do this is to debug the OCLC webpage code.  All main browsers all have excellent developer tools available, and if you run a network scan while using the page it's easy to see that the relevant data is all being loaded dynamically by calling to web services when different options are selected (e.g. selecting a particular country).  When selecting to view the data for Afghanistan, the following URL is accessed, which returns XML data:

[http://oclc.org/apps/oclc/gls?action=/service/location/AF](http://oclc.org/apps/oclc/gls?action=/service/location/AF)

This rules out using the option of using YQL to query the HTML contents of the page, as the data won't have been loaded until certain user interaction.  But having an XML document for each location is easier - the data could be downloaded for each country and imported directly into a spreadsheet or database. Yet even that is a little too dull: hard-coding a list of hundreds of country codes and importing the XML manually?

The country codes themselves are also stored in the form of a web service which returns XML.  These are queried by region, one of the regions the OCLC uses is *EMEA*: Europe, the Middle East, and Africa.

[http://oclc.org/apps/oclc/gls?action=/service/region/EMEA/type/OCLC](http://oclc.org/apps/oclc/gls?action=/service/region/EMEA/type/OCLC)

A solution could automatically download all the country codes for each region and use them to load the stats.  To ensure it doesn't hammer the website too hard, and results are returned relatively quickly, in this tutorial it'll just be the EMEA region (still 130 or so countries).

### step 3: querying the country codes 
The first thing the YQL query will need is the country codes, gained by querying the URL above.  In the YQL console:
<pre class="prettyprint linenums">
<code class="language-html">select * from xml 
where url = 'http://oclc.org/apps/oclc/gls?action=/service/region/EMEA/type/OCLC'
and itemPath='libstatsRegion.libstatsLocation.ISO_code'</code>
</pre>

To anyone familiar with SQL (structured query language) this will look both familiar and odd.  The structure looks the same, but it's perhaps not what would be expected.  If you were writing a pseudo-SQL query to represent querying the data from a URL you would expect something more like: *select libstatsRegion.libstatsLocation.ISO_code
from 'http://oclc.org/apps/oclc/gls?action=/service/region/EMEA/type/OCLC'*

There are quirks to YQL that it takes some time getting used to.  The *tables* being queried are dynamic ones that specify the type of data (as in *from xml*); the contents of those tables are dependant upon the where clause (*where url=...*).

Back to the task, the query above returns all the country codes, a snippet of the results show below:

<pre class="prettyprint linenums">
<code class="language-html">&lt;ISO_code xmlns=&quot;http://www.oclc.org/libstats&quot;&gt;AF&lt;/ISO_code&gt;
&lt;ISO_code xmlns=&quot;http://www.oclc.org/libstats&quot;&gt;AX&lt;/ISO_code&gt;
&lt;ISO_code xmlns=&quot;http://www.oclc.org/libstats&quot;&gt;AL&lt;/ISO_code&gt;
&lt;ISO_code xmlns=&quot;http://www.oclc.org/libstats&quot;&gt;DZ&lt;/ISO_code&gt;
&lt;ISO_code xmlns=&quot;http://www.oclc.org/libstats&quot;&gt;AD&lt;/ISO_code&gt;
&lt;ISO_code xmlns=&quot;http://www.oclc.org/libstats&quot;&gt;AO&lt;/ISO_code&gt;</code>
</pre>

As with SQL it would be possible to sort and filter the data (such as all items beginning with A...).

### step 4: modify the query to construct URLs
We can get a list of country codes, but what is really required is a list of URLs that include the country code, to get the stats.  This is done using the YQL *urltemplate* functionality.  A query is constructed which constructs a URL based on the results of a sub-query.

<pre class="prettyprint linenums">
<code>select * 
from uritemplate 
where template="http://oclc.org/apps/oclc/gls?action=/service/location/{item}" 
and item in (
    select ISO_code
    from xml 
    where url = 'http://oclc.org/apps/oclc/gls?action=/service/region/EMEA/type/OCLC' 
    and itemPath='libstatsRegion.libstatsLocation.ISO_code')</code>
</pre>

Running that in the console returns a list of URLs that we know will return library stats for each country.

<pre class="prettyprint linenums">
<code class="language-html">&lt;url&gt;&lt;![CDATA[http://oclc.org/apps/oclc/gls?action=/service/location/AF]]&gt;&lt;/url&gt;
&lt;url&gt;&lt;![CDATA[http://oclc.org/apps/oclc/gls?action=/service/location/AX]]&gt;&lt;/url&gt;
&lt;url&gt;&lt;![CDATA[http://oclc.org/apps/oclc/gls?action=/service/location/AL]]&gt;&lt;/url&gt;
&lt;url&gt;&lt;![CDATA[http://oclc.org/apps/oclc/gls?action=/service/location/DZ]]&gt;&lt;/url&gt;</code>
</pre>

Building up sub-queries gives YQL a lot of power, and it helps when constructing queries to create them step by step.  In this case, it's first finding out how to get all country codes, then using the result of that to construct a list of URLs.  

### step 5: query all the URLs
Finally, it's possible to use that list of URLs as a further sub-query to select the data from all of those sources.  This is done fairly simply by using the *in* operator rather than *=*, which allows us to specify a list of URLs.

The final query becomes:

<pre class="prettyprint linenums">
<code>select *
from xml
where url in (
    select url
    from uritemplate
    where template="http://oclc.org/apps/oclc/gls?action=/service/location/{item}"
    and item in (
        select ISO_code
        from xml 
        where url = 'http://oclc.org/apps/oclc/gls?action=/service/region/EMEA/type/OCLC'
        and itemPath='libstatsRegion.libstatsLocation.ISO_code'))</code>
</pre>

The YQL console allows an option to generate the url that would be required to access that query directly, and the format you wish the data in (XML or JSON).

There are better examples to demonstrate the power of YQL, but the amount that is being done in that one query - to retrieve a list of countries, use those to query another 130 URLs for data, to combine all that data and to provide it for use on any website in multiple formats seems beautifully easy.  And when pulling in data to use on a website, it saves a lot of code that would need to be run either on the web server or the client's browser.

## in action
A table below shows this in action, displaying data from the EMEA region.  Occasionally this fails to load when the data doesn't return in time for YQLs liking but a page refresh should sort that out.