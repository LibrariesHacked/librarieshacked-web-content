---
Title: creating data driven dashboards
Description: creating interactive charts that cross filter data to provide dashboards.
Date: 2015/05/05
Type: Tutorial
Author: dave
GitHub: 
TutorialComplexity: medium
TutorialSkillsRequired: JavaScript,DCJS,D3
TutorialNeeded: web browser,a text editor of your choice
TutorialTime: 1 hour
TutorialType: WebDevelopment
Template: tutorialpagedcjslending
---

Creating charts from data is a standard process using tools such as Excel, and there are many web publishing tools to create charts for the web (like [Highcharts](http://www.highcharts.com/), [Google Charts](https://developers.google.com/chart/), and [Dimple](http://dimplejs.org/)).

DCJS (Dimensional Charting JavaScript) goes a little further as it uses [Crossfilter](http://square.github.io/crossfilter/) which is a JavaScript library used to dynamically filter views on large datasets.  It also uses [D3 (data driven documents)](http://d3js.org/), which aids in creating beautiful visualisations from data using web standards.  Those two supporting technologies allow DC JS users to create the usual set of charts like other tools (bar, line, pie), but with enhanced cross-filtering functionality.  A demo of the kind of power of the charts is on the main DC JS homepage, visualising [Nasdaq 100 index data](http://dc-js.github.io/dc.js/).

An example
----------

The **Bath: Hacked** community data store holds data on library lending transactions for Bath from April 2013 to January 2014.  The data, available to download as CSV (or use an API connector), looks like this, with a row for each transaction **(Note: Looking at this closer it appears there's another filter on the data as there are far too few rows - will query this)**.

| Library name | CIPFA category | Category | Date (month) |
| ------------ | -------------- | -------- | ------------ |
| B&NES Mobile Libraries | Children's Fiction | Children's Picture Book | 1/04/2013 |
| Bath Central Library | Adult Fiction | Adult Graphic | 1/12/2013 |

Creating a data-driven dashboard is about giving users the ability to filter and query that data for themselves from their own questions, rather than creating fixed charts or views on the data.  In this example, questions could be:

- what does the trend of lending transactions look like for the central library compared to the mobile libraries?
- are there particular categories of lending item that people use council connect to deal with rather than a library?

The dataset itself is relatively straightforward so wouldn't pose much challenge to a manual data analysis.  But if that lending data were as detailed as being transactions per hour, over the course of the year, and even included data from the library accounts involved, the data could answer questions like:

- for only people who live in X ward, how many lending transactions are there for their local library, compared to the central library?  How does that comparison differ for people who live in Y ward?

That combined with other datasets such as deprivation by ward, or public transport/car ownership could start giving some real insight.  But regardless of the questions asked of the data, the dashboards themselves provide a means of answering questions using the relevant dimensions of the data.

Set up the HTML
---------------

The majority of 'work' with DC JS is done in JavaScript so the only HTML really required is placeholders for where the charts will appear, and including the relevant supporting scripts and CSS files.  The following template can be used to create a line, bar, and two pie charts.

<pre class="prettyprint">
<code>
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;title&gt;&lt;/title&gt;
    &lt;meta http-equiv="content-type" content="text/html; charset=UTF8"&gt;
    &lt;link rel="stylesheet" type="text/css" href="dc.css" media="screen" /&gt;

    &lt;script src="https://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.js" type="text/javascript"&gt;&lt;/script&gt;
    &lt;script src="http://d3js.org/d3.v3.min.js"&gt;&lt;/script&gt;
    &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/crossfilter/1.3.11/crossfilter.min.js"&gt;&lt;/script&gt;
    &lt;script src="dc.min.js"&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id="chart-date-line"&gt;&lt;/div&gt;
    &lt;div id="chart-library-row"&gt;&lt;/div&gt;
    &lt;div id="chart-category-bar"&gt;&lt;/div&gt;
    &lt;div id="chart-type-pie"&gt;&lt;/div&gt;
    &lt;div id="chart-librarytype-pie"&gt;&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
</code>
</pre>

Load the data using JavaScript
------------------------------

Having a CSV (comma separated value) file makes it easy to import the data to be used by JavaScript.  D3 is used to load in the data, in this case just by referring to the file.

<pre class="prettyprint"><code>d3.csv("LibraryLendingTransactions.csv", function (data) {
    // contents of the CSV can now be accessed through the 'data' variable
    // the dates are in a specific format which we need to tell D3
    var parseDate = d3.time.format("%m/%d/%Y 12:00:00 AM").parse;
    // then for each row, format the date to a date variable (from IssueDate column)
    data.forEach(function (d) {
        d.date = parseDate(d.IssueDate);
    });
    // set up crossfilter on the data.
    var ndx = crossfilter(data);
});</code></pre>

Set up the dimensions
---------------------

The dimensions exposed to the charts can just be the columns of the data source (in this case item category, month, library name), but can also be derived dimensions from those columns.  For example if item categories could be further split into adult/children then that could be a separate dimension on the data (without getting into the relative merits of that grouping).

In the code example below, the main columns are created as dimensions, and also an adult/child/uncategorised dimension based on the existence of *children* or *adult* being present in the category name.  Also there is a dimension to split the library name column into one of mobile/static/council/web.

<pre class="prettyprint"><code>// set up the dimensions
var dateDim = ndx.dimension(function (d) { return d.date; });
var categoryDim = ndx.dimension(function (d) { return d.CIPFACategoryName; });
var typeDim = ndx.dimension(function (d) {
    if (d.CIPFACategoryName.indexOf('Adult') != -1) return 'Adult';
    if (d.CIPFACategoryName.indexOf('Children') != -1) return 'Children';
    return 'Undefined';
});
var libraryDim = ndx.dimension(function (d) { return d.LibraryName; });
var libraryTypeDim = ndx.dimension(function (d) {
    if (d.LibraryName.indexOf('Mobile') != -1) return 'Mobile';
    if (d.LibraryName.indexOf('Web') != -1) return 'Web';
    if ((d.LibraryName.indexOf('Council') != -1) || (d.LibraryName.indexOf('Callpoint') != -1)) return 'Council';
    return 'Static';
});</code></pre>

Set up the values
-----------------

In the case of this data, the values that we are plotting are really just the number of rows that occur for each dimension filter.   It could be that values come from actual data in the rows though, in that case there are options to take numerical values and create a sum, average etc.

<pre class="prettyprint"><code>// set up the groups/values
var dateDimGroup = dateDim.group();
var categoryDimGroup = categoryDim.group();
var typeDimGroup = typeDim.group();
var libraryTypeDimGroup = libraryTypeDim.group();
var libraryGroup = libraryDim.group();</code></pre>

Create the charts
-----------------

Once those things are done the charts are initialised using a set of options to control the height, width, colours, axis scales, and many more (most importantly the relevant dimension and group to use for each chart).  The [documentation for DC JS](https://github.com/dc-js/dc.js/blob/master/web/docs/api-latest.md) is comprehensive but it can be more useful just to look through various examples and see how they've been created.

<pre class="prettyprint"><code>// the 5 different charts - options are set below for each one.
var libraryChart = dc.rowChart('#chart-library-row');
var totalLine = dc.lineChart("#chart-date-line");
var categoryChart = dc.barChart("#chart-category-bar");
var type = dc.pieChart("#chart-type-pie");
var libraryType = dc.pieChart("#chart-librarytype-pie");
libraryChart
  .width(300)
  .height(300)
  .group(libraryGroup)
  .dimension(libraryDim)
  .elasticX(true)
  .xAxis().ticks(4);

categoryChart
  .width(300)
  .height(200)
  .outerPadding(0)
  .gap(1)
  .margins({ top: 0, right: 0, bottom: 95, left: 30 })
  .group(categoryDimGroup)
  .dimension(categoryDim)
  .elasticY(true)
  .xUnits(dc.units.ordinal)
  .brushOn(false)
  .x(d3.scale.ordinal())
  .renderHorizontalGridLines(true);

categoryChart.on("postRender", function (c) {
    d3.selectAll('#chart-category-bar .axis.x text').style("text-anchor", "end").attr("transform", function (d) { return "rotate(-90, -4, 9) "; });
});

totalLine
  .width(490)
  .height(200)
  .margins({ top: 5, right: 10, bottom: 20, left: 50 })
  .renderArea(true)
  .group(dateDimGroup, "Count of transactions")
  .elasticX(true)
  .elasticY(true)
  .dimension(dateDim)
  .renderHorizontalGridLines(true)
  .brushOn(false)
  .yAxisPadding("20%")
  .x(d3.time.scale().domain([minDate, maxDate]));

type
  .width(180)
  .height(180)
  .radius(80)
  .dimension(typeDim)
  .group(typeDimGroup)
  .renderLabel(true)
  .innerRadius(10)
  .transitionDuration(500)
  .colorAccessor(function (d, i) { return d.value; });

libraryType
  .width(180)
  .height(180)
  .radius(80)
  .dimension(libraryTypeDim)
  .group(libraryTypeDimGroup)
  .renderLabel(true)
  .innerRadius(10)
  .transitionDuration(500)
  .colorAccessor(function (d, i) { return d.value; });

// at the end this needs to be called to actually go through and generate all the graphs on the page.
dc.renderAll();</code></pre>

Sources
-------

- A set of [simple, specific examples](http://dc-js.github.io/dc.js/examples/) for DC JS.
- For an excellent set of examples of using just D3 for data visualisations see the [D3 GitHub example page](https://github.com/mbostock/d3/wiki/Gallery).