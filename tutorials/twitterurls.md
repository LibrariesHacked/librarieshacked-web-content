---
Title: extracting link data from twitter searches
Description: creating a summary of links from twitter hashtags, for revisiting later.
Template: tutorialpagetwitterurls
Type: Tutorial
Date: 2014/08/1
Author: @librarieshacked
TutorialType: WebDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: jquery,javascript,json,sql
TutorialNeeded: an HTML editor of your choice
TutorialTime: 2 hours
---

## what's the problem?

When following hashtags on twitter for conferences or chats there are often a lot of links being thrown about.  With a stream of tweets going on it's normally necessary to save these for later.  You can favourite the tweets, although this relies on catching the tweet at the time.  Or search twitter later, although as far as a summary goes there may be loads of links - and the tweets don't always give a good enough context to show more information about what the link contains.

The web being as clever as it is though, it should be possible to list all the links shared through a hashtag, alongside additional information about them - gained directly from the web pages themselves, such as the page title and description.

Yahoo query language (YQL) allows for directly searching the html of web pages, and also provides access to searching twitter.  This post will do the following (an example is at the top of the page):

- use YQL to query twitter for a particular search term (e.g. a hashtag)
- retrieve those tweets that contain URLs and compile them to remove duplicates, record the user, and other context such as the tweet text.
- pass the set of de-duplicated URLs back into YQL to get meta information from the web pages such as page title and description.
- merge this data all together and display it in on a webpage.

It sounds quite a lot in terms of programming, but using YQL and jQuery to shorten the amount of JavaScript, it can be done in a very small amount of code.  If you are completely unfamiliar with YQL, it was partly covered in this example of [collecting global library stats](http://www.librarieshacked.org/tutorials/yqlstats).  There is also an excellent [developer resource](https://developer.yahoo.com/yql/).

## step 1: search twitter for tweets and links

YQL comes with some *community tables*.  In [the YQL console](https://developer.yahoo.com/yql/console/) you can tick *show community tables* on the left hand side and you get access to all kinds of data: wikipedia, twitter, amazon, bbc, etc.  If you select the twitter tables and select *twitter.search.tweets* an example is generated of a twitter search. this appears as:

<pre class="prettyprint linenums">
<code>SELECT * FROM twitter.search.tweets 
WHERE q="yahoo" 
AND consumer_key="08ZNcNfdoCgYTzR7qcW1HQ" 
AND  consumer_secret="PTMIdmhxAavwarH3r4aTnVF7iYbX6BRfykNBHIaB8"
AND access_token="1181240586-JIgvJe4ev3NHdHnAqnovHINWfpo0qB2S2kZtVRI" 
AND access_token_secret="1nodv0LBsi7jS93e38KiW8cHOA5iUc6FT4L6De7kgk"</code>
</pre>

In a production system those consumer keys and access tokens would be your own, provided by twitter when you create a developer account.  The following article gives instructions of how to create those keys and hide them when using YQL within a web page: [Twitter API v1.1 Front-end Access](http://stevezeidner.com/twitter-api-v1-1-front-end-access-with-yql/)

YQL then makes querying twitter quite straightforward, and after designing a query in the console it provides a direct link to access the data returned.  Taking a library related query as an example, a query on *savelibraries* could be run by accessing the following URL, set to return JSON data:

[https://query.yahooapis.com/v1/public/yql?q=select...](https://query.yahooapis.com/v1/public/yql?q=select%20*%20FROM%20twitter.search.tweets%20WHERE%20q%3D%22savelibraries%22%20AND%20consumer_key%3D%2208ZNcNfdoCgYTzR7qcW1HQ%22%20AND%20consumer_secret%3D%22PTMIdmhxAavwarH3r4aTnVF7iYbX6BRfykNBHIaB8%22%20AND%20access_token%3D%221181240586-JIgvJe4ev3NHdHnAqnovHINWfpo0qB2S2kZtVRI%22%20AND%20access_token_secret%3D%221nodv0LBsi7jS93e38KiW8cHOA5iUc6FT4L6De7kgk%22&format=json&diagnostics=true&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys&callback=)

In one go that returns tweets matching that search term, any linked URLs, user data for whoever sent each tweet, and location if attached, etc.

## step 2: get the metadata for the links within tweets
The data returned includes links from within tweets that are already separated from the text of the tweet, so there's no need to mess around detecting them or manipulating the text data to extract them.

The next task is then to provide additional information for those links by querying YQL for meta tags within the html of pages.  A YQL query to return the meta and title tags from a web link would be:

<pre class="prettyprint linenums">
<code>SELECT *
FROM html
WHERE url = 'http://www.librarieshacked.org'
AND xpath='//head/meta|//head/title'</code>
</pre>

The *xpath* query there defines which parts of the html are queried.  In this case, the query will return data from any meta tags that appear in the head section of the html, and the title tag.  YQL also allows for combining many queries at a time, by specifying these in the following way:

<pre class="prettyprint linenums">
<code>SELECT *
FROM yql.multi
WHERE queries = "
    SELECT * FROM html WHERE url = 'http://www.librarieshacked.org' AND xpath='//head/meta|//head/title';SELECT * FROM html WHERE url = 'http://www.librarieshacked.org/tutorials' AND xpath='//head/meta|//head/title'"</code>
</pre>

## step 3: use JavaScript/jQuery to put it all together
To effectively use those two data sources, a tool needs to be created which will take a search term, find all the tweets that include links, use these to retrieve the webpage meta tags, and display this. Using the jQuery library, an example JavaScript function is:

<pre class="prettyprint linenums">
<code>function GetResults(search) {
    // empty out the results
    // divUrlResults is the 'output' container
    $('#divUrlResults').empty();

    // construct url to get tweet data
    var yqlGetTweets = "SELECT statuses FROM twitter.search.tweets WHERE q='"
            + search
            + "' AND consumer_key='08ZNcNfdoCgYTzR7qcW1HQ' "
            + "AND consumer_secret='PTMIdmhxAavwarH3r4aTnVF7iYbX6BRfykNBHIaB8' "
            +"AND access_token='1181240586-JIgvJe4ev3NHdHnAqnovHINWfpo0qB2S2kZtVRI' "
            +"AND access_token_secret='1nodv0LBsi7jS93e38KiW8cHOA5iUc6FT4L6De7kgk'"
    var yqlTweetsUrl = "https://query.yahooapis.com/v1/public/yql?q="
            + encodeURI(yqlGetTweets)
            + "&format=json&env=store://datatables.org/alltableswithkeys";

    // make the call to yql and wait for it to return (success:...)
    $.ajax({
        url: yqlTweetsUrl,
        dataType: "jsonp",
        success: function (data) {
            // on success set up an array of data to store each url urlData[]
            var urlData = [];
            // and a set of select statements for later call to yql
            var yqlSelects = "";

            if (data && data.query && data.query.count
                    && data.query.count > 0 && data.query.results.json) {
                // for each tweet returned process it
                $.each(data.query.results.json, function (key, value) {
                    if (this.statuses.entities && this.statuses.entities.urls) {
                        // create an object with relevant data from the tweet
                        var tweet = {
                            text: this.statuses.text, username: this.statuses.user.name,
                            usertag: this.statuses.user.screen_name, link: '',
                            tweetdate: this.statuses.created_at
                        };
                        $.isArray(this.statuses.entities.urls) ? tweet.link = this.statuses.entities.urls[0].expanded_url : tweet.link = this.statuses.entities.urls.expanded_url;
                        // checking if url has already been added (not adding duplicates)
                        if ($.grep(urlData, function (e) { return e.link == tweet.link; }).length == 0
                            && tweet.link.length > 0 && urlData.length < 5) {
                            var description;
                            urlData.push(tweet);
                            yqlSelects += "select * from html where url = '"
                                + tweet.link + "' and xpath='//title|//head/meta';"
                        }
                    }
                });

                // construct url to get web page meta data
                var yqlGetMeta = "select * from yql.query.multi where queries = \""
                    + yqlSelects + "\"";
                var yqlMetaUrl = "https://query.yahooapis.com/v1/public/yql?q="
                    + encodeURI(yqlGetMeta) + "&format=json";

                // make the call to yql and wait for it to return (success:...)
                $.ajax({
                    url: yqlMetaUrl,
                    dataType: "jsonp",
                    success: function (data) {
                        if (data && data.query && data.query.results && data.query.count > 0) {
                            // for each item returned add a new section on the web page to show it.
                            $.each(data.query.results.results, function (i) {
                                $('#divUrlResults').append("&lt;h4&gt;" + this.title
                                        // cnstruct the html for each item
                                        + "&lt;/h4&gt;&lt;h5&gt;" + description + "&lt;/h5&gt;&lt;a href='"
                                        + urlData[i].link + "'&gt;" + urlData[i].link
                                        + "&lt;/a&gt;&lt;p&gt;tweeted by "
                                        + urlData[i].username + " at " + urlData[i].tweetdate
                                        + "&lt;/p&gt;&lt;hr&gt;");
                            });
                        }
                    }
                });
            }
        }
    });
}</code>
</pre> 

That JavaScript will then search twitter, find some additional information about the shared links, and populate a *div* on the page with that data.  It could used to populate a twitter style widget, showing links shared rather than tweets, with summary data.