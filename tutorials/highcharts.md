---
Title: making interactive web charts with Highcharts
Description: linking to CSV data to quickly publish live charts on the web.
Type: Tutorial
Tags: 
Template: tutorialpagehighcharts
Author: dave
Date: 2015/11/30
GitHub: 
TutorialType: WebDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: JavaScript,Highcharts
TutorialNeeded: web browser
TutorialTime: 1 hour
---

The [tools page](https://www.librarieshacked.org/tools) on this site gives some examples of tools for data analysis and data visualisation.  There are too many to list, but [HighCharts](http://www.highcharts.com/) is an especially easy charting library to create a wide variety of interactive charts with relatively little coding knowledge.

Note: HighCharts is free for non-commercial and personal use.  For commercial use (which includes government), it does require a developer licence.

[Leeds Data Mill](http://leedsdatamill.org/) is one of the UK's leading local open data portals, publishing a wide variety of datasets about the city. They publish a set of library-based datasets, providing annual usage counts (in some cases since 2000), separated by individual library.

- [Library visits](http://leedsdatamill.org/dataset/library-visits) 
- [Library loans (books)](http://leedsdatamill.org/dataset/library-loans-books-only)
- [Library loans (all media)](http://leedsdatamill.org/dataset/library-loans-all-media)
- [Library computer bookings](http://leedsdatamill.org/dataset/library-computer-bookings)

These are downloadable as CSV files, but often in cases where datasets may be updated at source it is useful to create *live* visualisations pointing to the data rather than downloading a copy.

This tutorial takes the library computer bookings dataset as an example and uses HighCharts to create a line chart visualisation, showing the computer bookings counts for each library from 2001 to 2014. An example of the chart, created with the same code and demonstrating the interactive features of HighCharts, is at the top of this page.

### Step 1.  Set up the base HTML

To start from scratch (rather than embedding into an existing page), create a blank HTML file which will include:

- Bootstrap CSS/JavaScript references, and jQuery.  This is optional but gives the page a bit of structure and automatically responsive design.  When quickly 'mocking up' pages it can be a useful first step to producing elegant pages before beginning extra design work.
- A &lt;div&gt; container with a particular ID (in this case 'chart').  This is where Highcharts will create the chart.
- A link to the HighCharts JavaScript file (hosted at highcharts.com).
- A link to the Papa Parse JavaScript file (more on that later) - this is used to process the CSV data.

<pre class="prettyprint linenums"><code>&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="utf-8"&gt;
    &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1"&gt;
    &lt;title&gt;Library IT usage data visualisation&lt;/title&gt;
    &lt;link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css"&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class="container"&gt;
        &lt;h1&gt;Usage data&lt;/h1&gt;
        &lt;div id="chart" style="height: 600px"&gt;&lt;/div&gt;
    &lt;/div&gt;
    &lt;script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"&gt;&lt;/script&gt;
    &lt;script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"&gt;&lt;/script&gt;
    &lt;script src="https://code.highcharts.com/highcharts.js"&gt;&lt;/script&gt;
    &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/4.1.2/papaparse.min.js"&gt;&lt;/script&gt;
    &lt;script&gt;
    // JavaScript code will go here
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>

### Step 2.  Write JavaScript to create the chart

Once the structure of the page is in place, there is some JavaScript code needed to create the chart when the page has loaded.

The code needs the data from the CSV file.  [Papa Parse](http://papaparse.com/) (included on the page in previous step) provides a single method (*parse*) for loading in remote CSV data and converting it to a data array that can be used by the HighCharts code. The code (commented below) becomes relatively short to create the fully interactive chart.  The steps are:

- Parse the CSV file using Papa Parse.
- Once complete, create an array of data to represent the X Axis categories (the years).
- Create an array of series object that represent the data values for each library.
- On the chart div container (*$('#chart')*) create the chart with some optional text values and the data.

<pre class="prettyprint linenums"><code>$(function () {
    // PapaParse takes the link to the CSV file, hosted at Leeds Data Mill
    Papa.parse('https://aql.datapress.com/leeds/dataset/library-computer-bookings/library-it-use.csv', {
        download: true,
        complete: function (results) {
            // The set of years (X-Axis) are in the first row of data (use data[0]), except the header row (use slice(1)).
            // This creates a category array that looks like ['2001/2002', '2002/2003', '...' ]
            var years = results.data[0].slice(1);
            // Create an array of series objects.
            // Each one will be of the form { name: 'Series name', data: [1,2,3,4,5] }
            // The data values will match the number of categories (years)
            var series = [];
            // Each row except the header (use slice(1) again) is a set of data for an individual library
            results.data.slice(1).forEach(function (item, index) {
                // For each column in the individual library row return it as a number (replacing the N/A values with zeros)
                var data = item.slice(1).map(function (val) {
                    return val != 'N/A'? Number(val) : null;
                });
                // Add the library to the series array
                // Because there are so many libraries, the visibility of the library is only set for the first two
                // The user can then pick and choose which libraries to show.
                series.push({ name: item[0], data: data, visible: (index &lt;= 2) });
            });

            // Now we have the data setup, construct the chart with some text options.
            // The series array is set as the series variable.
            // The years array is set as the X axis categories
            $('#chart').highcharts({
                title: { text: 'Leeds libraries IT usage' },
                subtitle: { text: 'Data from Leeds Data Mill' },
                xAxis: { categories: years },
                yAxis: { title: { text: 'Computer sessions' } },
                series: series
            });
        }
    });
});</code></pre>
