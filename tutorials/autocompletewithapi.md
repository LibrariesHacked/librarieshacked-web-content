---
Title: book search auto-complete
Description: auto-completing text boxes for book search suggestions
Type: Tutorial
Tags: 
Template: tutorialpageautocomplete
Author: dave
Date: 2014/07/05
GitHub: tutorial_autocomplete
TutorialType: WebDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: HTML,JavaScript,jQuery,JSON
TutorialNeeded: an HTML text editor of your choice
TutorialTime: 1 hour
---

Open <abbr title="Application Programming Interfaces">APIs</abbr> (Application Programming Interfaces) provide opportunities to create a wide variety of applications, reusing systems and data for new tools. But sometimes these can also just provide small user-experience functions. This tutorial will cover using an API to develop auto-complete functionality on a website.

#### Why use auto-complete?

A book search is a good example where auto-complete can be useful. When using the catalogue search on library sites the steps may be:

1. Enter the search term either for an author, or a title. If author it must be in the form **surname, first name**.  Hit search.
2. Scroll through list of search results (**page 3 of 180...**). If too many results, you can go back the original screen and modify your search. If no results the results page will be empty.

That doesn't give users a speedy experience. Using auto-complete allows you to show data as the user types. As more is typed, the results are refined by the increased detail. Once the user sees the option they want, they can select it.  The underlying data could come from many sources. Sometimes, like when using Google, it's common searches that other people have performed. Other times, it's actually looking at the underlying database, and giving an indication if there are any matches.

This kind of dynamic search information can prompt the user for what they're looking for (if they have half a title in their mind). It can also be used to provide them details of their search results before they've even pressed search. There is often no point in showing a results page of zero results, unless an easy option to adjust the search is provided Could the user be informed before they have run the search that no results will be returned?

The [Google Books API](http://books.google.co.uk/) is one API to provide book searching. There are plenty of others though including [open library](http://openlibrary.org). The Google Books API should normally be used with an API key, which is granted by Google and needs to be included in the URL when a search is made.

#### Step 1. Start with a basic page template

The example will be coded within a single web page; the following is a basic HTML5 page template to get started.

<pre class="prettyprint linenums"><code class="language-html">&lt;!doctype html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;utf-8&quot;&gt;
    &lt;title&gt;auto-complete API tutorial&lt;/title&gt;
    &lt;meta name=&quot;description&quot; content=&quot;auto-complete API tutorial&quot;&gt;
    &lt;meta name=&quot;author&quot; content=&quot;libraries hacked&quot;&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>

#### Step 2. Add references to JavaScript and CSS files

For this tutorial we need jQuery and jQuery UI to provide the auto-complete functionality. These are JavaScript **libraries**, created to simplify common JavaScript tasks, making them quicker and easier to code. They are added to the page as shown below:

<pre class="prettyprint linenums"><code class="language-html">&lt;head&gt;
    &lt;meta charset=&quot;utf-8&quot;&gt;
    &lt;title&gt;auto-complete API tutorial&lt;/title&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;//code.jquery.com/ui/1.10.4/themes/smoothness/jquery-ui.css&quot;&gt;
    &lt;meta name=&quot;description&quot; content=&quot;auto-complete API tutorial&quot;&gt;
    &lt;meta name=&quot;author&quot; content=&quot;libraries hacked&quot;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;script src=&quot;//code.jquery.com/jquery-1.10.2.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;//code.jquery.com/ui/1.10.4/jquery-ui.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;script.js&quot;&gt;&lt;/script&gt;
&lt;/body&gt;</code></pre>

The files referred to here are held at **code.jquery.com**. These could be hosted as part of the project, but there are advantages to using externally hosted versions:

- Users already having visited a site referencing those same JavaScript/CSS files won't need to download them again. Their browser will cache them, effectively speeding up your site and saving the user time and data usage.
- The files are hosted on web servers where performance is optimised, likely faster than your web server. They are often distributed on many servers.
- Less file space is used on your web server, and less cost to your bandwidth allowances.

You do need to ensure you have a web connection when running the page on your own computer. In this case the tutorial will need web access anyway, to communicate with the API.

In the HTML a new file is also referenced, **script.js**. This will contain the custom JavaScript code, to be written in part 4.

#### Step 3. Add the textbox HTML

The web page will just have a header and an input text box, and a space where details of the book will be added dynamically later.

<pre class="prettyprint linenums"><code class="language-html">&lt;body&gt;
    &lt;h1&gt;auto-complete book search example&lt;/h1&gt;
    &lt;p&gt;
        &lt;label for=&quot;txtBookSearch&quot;&gt;select a book&lt;/label&gt;
        &lt;input id=&quot;txtBookSearch&quot; type=&quot;text&quot;&gt;
    &lt;/p&gt;
        &lt;h2&gt;book details&lt;/h2&gt;
    &lt;div id=&quot;divDescription&quot;&gt;&lt;/div&gt;
    &lt;script src=&quot;//code.jquery.com/jquery-1.10.2.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;//code.jquery.com/ui/1.10.4/jquery-ui.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;js/scripts.js&quot;&gt;&lt;/script&gt;
&lt;/body&gt;</code></pre>

### Step 4. Add the auto-complete JavaScript

The following code is added to the empty **script.js** file, providing a framework for the autocomplete functionality, which is part of JQuery UI.

<pre class="prettyprint linenums"><code class="language-javascript">$(document).ready(function () {  //should only run once the document is ready
    $('#txbBookSearch').autocomplete({ // attach the autocomplete functionality to the textbox
        source: function (request, response) {
            // define the source of the data
        },
        select: function (event, ui) {
            // what to do when an item in the autocomplete list is selected
        },
        minLength: 2 // set the minimum length of text the user must enter
    });
});</code></pre>

Having jQuery on the page allows for the text box to be referenced by using it's ID (**$('#txbBookSearch')**).  jQueryUI gives the functionality to easily attach to the text box so that it will provide suggestions as text is entered (**.autocomplete(....)**).

The full auto-complete documentation is available on the [jQueryUI pages](http://jqueryui.com/autocomplete). In this basic template there is space to write the code that will run at two different **events**:

- When the input text is changed, in order to provide the suggestions (**source: function (request, response)...**);
- When an item is selected from the suggestions (**select: function (event, ui)...**);

There is also the option to specify the number of characters that have to be entered before it will start making suggestions (**minLength:...**). This example sets it to 2. That's a little low, so in reality you may want more.

#### Step 5. Provide search suggestions

When a user enters text of at least 2 characters, the code should search all books from the google API, and list the closest matches.

<pre class="prettyprint linenums"><code class="language-javascript">$(document).ready(function () {  // only begin once page has loaded
    $('#txtBookSearch').autocomplete({ // attach auto-complete functionality to textbox
        // define source of the data
        source: function (request, response) {
            // url link to google books, including text entered by user (request.term)
            var booksUrl = 'https://www.googleapis.com/books/v1/volumes?printType=books&amp;q=' + encodeURIComponent(request.term);
            $.ajax({
                url: booksUrl,
                dataType: 'jsonp',
                success: function(data) {
                    response($.map(data.items, function (item) {
                        if (item.volumeInfo.authors && item.volumeInfo.title 
                        && item.volumeInfo.industryIdentifiers && item.volumeInfo.publishedDate) {
                            return {
                                // label value will be shown in the suggestions
                                label: item.volumeInfo.title + ', ' + item.volumeInfo.authors[0] + ', ' + item.volumeInfo.publishedDate,
                                // value is what gets put in the textbox once an item selected
                                value: item.volumeInfo.title,
                                // other individual values to use later
                                title: item.volumeInfo.title,
                                author: item.volumeInfo.authors[0],
                                isbn: item.volumeInfo.industryIdentifiers,
                                publishedDate: item.volumeInfo.publishedDate,
                                image: (item.volumeInfo.imageLinks == null ? '' : item.volumeInfo.imageLinks.thumbnail),
                                description: item.volumeInfo.description,
                            };
                        }
                    }));
                }
            });
        },
        select: function (event, ui) {
            // what to do when an item is selected
        },
        minLength: 2 // set minimum length of text the user must enter
    });
});</code></pre>

The code added to call the API uses [jQuery's AJAX method](http://api.jquery.com/jquery.ajax/), which allows developers to send a call to a url for data, and specify what will happen once that data is returned.

The code transforms the data returned into a set of values that contain various details about each book: title, author, published date etc. By default the value we define as being the *label* value is the one shown within the drop down suggestion list. That label can be constructed from the returned values, so we can show 'title, author, date published' in the suggestions.

### Step 6. Selecting the book

All that's left is to code what will happen when a book is selected. This will depend on the site using the service - it could list all details of the book, or take the user to a page about the book in order to provide further actions. To demonstrate an example usage, this will show some details about the book on the page and provide a link to the OCLC WorldCat page for that book.

Full JavaScript code:

<pre class="prettyprint linenums"><code class="language-javascript">$(document).ready(function () { // only begin once page has loaded
    $('#txtBookSearch').autocomplete({ // attach auto-complete functionality to textbox
        // define source of the data
        source: function (request, response) {
            // url link to google books, including text entered by user (request.term)
            var booksUrl = 'https://www.googleapis.com/books/v1/volumes?printType=books&amp;q=' + encodeURIComponent(request.term);
            $.ajax({
                url: booksUrl,
                dataType: 'jsonp',
                success: function(data) {
                    response($.map(data.items, function (item) {
                        if (item.volumeInfo.authors && item.volumeInfo.title
                        && item.volumeInfo.industryIdentifiers && item.volumeInfo.publishedDate) {
                            return {
                                // label value will be shown in the suggestions
                                label: item.volumeInfo.title + ', ' + item.volumeInfo.authors[0] + ', ' + item.volumeInfo.publishedDate,
                                // value is what gets put in the textbox once an item selected
                                value: item.volumeInfo.title,
                                // other individual values to use later
                                title: item.volumeInfo.title,
                                author: item.volumeInfo.authors[0],
                                isbn: item.volumeInfo.industryIdentifiers,
                                publishedDate: item.volumeInfo.publishedDate,
                                image: (item.volumeInfo.imageLinks == null ? '' : item.volumeInfo.imageLinks.thumbnail),
                                description: item.volumeInfo.description
                            };
                        }
                    }));
                }
            });
        },
        select: function (event, ui) {
            // what to do when an item is selected
            // first clear anything that may already be in the description
            $('#divDescription').empty();
            // we get the currently selected item using ui.item
            // show a pic if we have one
            if (item.image != '') {
                $('#divDescription').append('&lt;img src="' + ui.item.image + '" style="float: left; padding: 10px;"&gt;');
            }
            // and title, author, and year
            $('#divDescription').append('&lt;p&gt;&lt;b&gt;Title:&lt;/b&gt; ' + ui.item.title  + '&lt;/p&gt;');
            $('#divDescription').append('&lt;p&gt;&lt;b&gt;Author:&lt;/b&gt; ' + ui.item.author  + '&lt;/p&gt;');
            $('#divDescription').append('&lt;p&gt;&lt;b&gt;First published year:&lt;/b&gt; ' + ui.item.publishedDate  + '&lt;/p&gt;';
            // and the usual description of the book
            $('#divDescription').append('&lt;p&gt;&lt;b&gt;Description:&lt;/b&gt; ' + ui.item.description  + '&lt;/p&gt;');
            // and show the link to oclc (if we have an isbn number)
            if (ui.item.isbn && ui.item.isbn[0].identifier) {
                $('#divDescription').append('&lt;p&gt;&lt;b&gt;ISBN:&lt;/b&gt; '
                + ui.item.isbn[0].identifier + '&lt;/p&gt;');
                $('#divDescription').append('&lt;a href="http://www.worldcat.org/isbn/'
                + ui.item.isbn[0].identifier + '" target="_blank"&gt;View item on worldcat&lt;/a&gt;');
            }
        },
        minLength: 2 // set minimum length of text the user must enter
    });
});</code></pre>

An example text box is included on this page to try it out.