---
Title: book search auto-complete textbox
Description: a tutorial on creating an auto-completing text box for providing book search suggestions.
Date: 2014/07/05
Template: tutorialpageautocomplete
Type: Tutorial
TutorialType: WebDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: HTML,JavaScript,jQuery,JSON
TutorialNeeded: an HTML text editor of your choice
TutorialTime: 1 hour
---

The number of open web APIs now available gives plenty of opportunity to create a wide variety of complete applications.  But sometimes these services can also be used in the background to provide small user experience improvements.  This tutorial will cover using an API to provide auto-complete functionality for a website text box.

## why use auto-complete?

A book search is a good example of where auto-complete functionality can be very useful.  When using library catalogue searches on sites, the steps to search for a book are often:

1. enter a search term either for an author, or a title.  if author it must be in the form **surname, first name**.  hit search.
2. scroll through list of search results (*page 3 of 180...*).  if it's returned too many results you can go back the original screen and modify your search.  if it's returned no results the results page will be empty

That doesn't give users an easy or quick experience.  Using auto-complete allows you to show results in a drop down list when the user enters text: as the user types, the results are refined by the increased detail.  Once the user sees the option that they want they can select it.

This kind of dynamic search information provided while the user is entering the search can both prompt the user for what they're looking for (if they have half a title in their mind), and it can also be used to provide them details of their search results before they've even pressed search.  There is often no point in showing a results page where there are none to display, unless an easy option to adjust the search is provided, or other useful information such as similar words and spelling corrections.  The information that a particular search term gave no results is useful to know, but not loading a whole new page that leaves the user only able to go back.

## services to use
The [Google Books API](http://books.google.co.uk/) is one option to look up books.  There are plenty of others though as well, [open library](http://openlibrary.org) for example could provide the same functionality.  The Google Books API should normally be used with an API key (which is granted by Google and needs to be included in the URL when a search is made).  They do show  examples without a key for testing, but the usage is throttled and user limits hit very quickly.  To use something like this in a production environment it would need an API key from google or a service such as worldcat search, or openlibrary (or whatever data source is appropriate for your autocomplete requirements).

## tutorial steps

### 1. start with a basic html5 template

The example will be done within a single web page, so the following is a basic html5 page template to get started.
<pre class="prettyprint linenums">
<code class="language-html">&lt;!doctype html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
	&lt;meta charset=&quot;utf-8&quot;&gt;
	&lt;title&gt;auto-complete API tutorial&lt;/title&gt;
	&lt;meta name=&quot;description&quot; content=&quot;auto-complete API tutorial&quot;&gt;
	&lt;meta name=&quot;author&quot; content=&quot;libraries hacked&quot;&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;/body&gt;
&lt;/html&gt;</code>
</pre>

### 2. add the required references to JavaScript and CSS files.

For this tutorial we need jQuery and jQuery UI to provide the auto-complete functionality.  these are *JavaScript libraries*, created to simplify common JavaScript tasks, making them quicker and easier to code.  They are added to the page as shown below:
<pre class="prettyprint linenums">
<code class="language-html">&lt;head&gt;
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
&lt;/body&gt;</code>
</pre>

The files referred to here are held at code.jquery.com.  These could be downloaded and included as part of the project code, but there are advantages to using externally hosted versions:

- your users who have already used a site referencing those same JavaScript/CSS files won't need to download them again - their browser will cache them, effectively speeding up your site and saving the user time and data usage.
- the files are hosted on web servers where performance is optimised and will be faster than most other web servers.
- less file space is used on your web server and less cost to your bandwidth allowances.

One thing to watch out for is that when using files held on the web, you then need to ensure you have a web connection when running the page on your local computer.  In this case the tutorial will need web access anyway to communicate with the API, so it's not worth taking local copies of the files.

In the HTML a new file is also referenced, **script.js**.  This will contain the custom JavaScript code written in part 4.

### 3. add the basic textbox html.

The web page itself will just have a header and an input text box, and a space where details of the book will be added later.
<pre class="prettyprint linenums">
<code class="language-html">&lt;body&gt;
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
&lt;/body&gt;</code>
</pre>
### 4. add the auto-complete JavaScript.
The following code is added to the empty script.js file, providing a framework for the jQuery UI autocomplete functionality.
<pre class="prettyprint linenums">
<code class="language-javascript">$(document).ready(function () {  // the code should only run once the document is ready
	$("#txbBookSearch").autocomplete({ // attach the autocomplete functionality to the textbox
        source: function (request, response) {
			// define the source of the data
        },
        select: function (event, ui) {
			// what to do when an item in the autocomplete list is selected
        },
        minLength: 2 // set the minimum length of text the user must enter
    });
});</code>
</pre>

Having jQuery on the page allows for the text box to be referenced easily by using it's ID (*$("#txbBookKeywork")*).  jQueryUI gives the functionality to easily attach to the text box so that it will provide suggestions as text is entered (*.autocomplete(....)*).

The full auto-complete documentation is available on the [jQueryUI pages](http://jqueryui.com/autocomplete/).  In this basic template there is a space to write custom code that will run at two different *events*:

- when the input text is changed, in order to provide the suggestions (*source: function (request, response)...*); 
- when an item is selected from the suggestions (*select: function (event, ui)...*);
- there is also the option to specify a number of characters that have to be entered before it will start making suggestions (*minLength:...*).

### 5. provide the search suggestions

When a user enters text of at least 2 characters, the code should search all books from the google API, and list the closest matches.
<pre class="prettyprint linenums">
<code class="language-javascript">$(document).ready(function () {  // only begin once page has loaded
	$("#txtBookSearch").autocomplete({ // attach auto-complete functionality to textbox
		// define source of the data
        source: function (request, response) {
			// url link to google books, including text entered by user (request.term)
			var booksUrl = "https://www.googleapis.com/books/v1/volumes?printType=books&amp;q=" + encodeURIComponent(request.term);
			$.ajax({
				url: booksUrl,
				dataType: "jsonp",
				success: function(data) {
                    response($.map(data.items, function (item) {
						if (item.volumeInfo.authors && item.volumeInfo.title && item.volumeInfo.industryIdentifiers && item.volumeInfo.publishedDate)
						{
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
								image: (item.volumeInfo.imageLinks == null ? "" : item.volumeInfo.imageLinks.thumbnail),
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
});</code>
</pre>

The code added to call the API uses [jQuery's AJAX method](http://api.jquery.com/jquery.ajax/), which allows developers to send a call to a url for data, and specify what will happen once that data is returned.

The code transforms the data returned into a set of values that contain various details about each book: title, author, published date etc. By default the value we define as being the *label* value is the one shown within the drop down suggestion list.  That label can be constructed from the returned values, so we can show 'title, author, date published' in the suggestions.

## 6. selecting the book

All that's left is to code what will happen when a book is selected.  This will depend on the site using the service - it could list all details of the book, or take the user to a page on the book in order to provide further actions.  To demonstrate an example usage, this will show some details about the book on the page and provide a link to the OCLC WorldCat page for that book.

Full JavaScript code:

<pre class="prettyprint linenums">
<code class="language-javascript">$(document).ready(function () {  // only begin once page has loaded
	$("#txtBookSearch").autocomplete({ // attach auto-complete functionality to textbox
		// define source of the data
        source: function (request, response) {
			// url link to google books, including text entered by user (request.term)
			var booksUrl = "https://www.googleapis.com/books/v1/volumes?printType=books&amp;q=" + encodeURIComponent(request.term);
			$.ajax({
				url: booksUrl,
				dataType: "jsonp",
				success: function(data) {
                    response($.map(data.items, function (item) {
						if (item.volumeInfo.authors && item.volumeInfo.title && item.volumeInfo.industryIdentifiers && item.volumeInfo.publishedDate)
						{
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
								image: (item.volumeInfo.imageLinks == null ? "" : item.volumeInfo.imageLinks.thumbnail),
								description: item.volumeInfo.description,
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
			if (item.image != '')
			{
				$('#divDescription').append('&lt;img src="' + ui.item.image + '" style="float: left; padding: 10px;"&gt;');
			}
			// and title, author, and year
			$('#divDescription').append('&lt;p&gt;&lt;b&gt;Title:&lt;/b&gt; ' + ui.item.title  + '&lt;/p&gt;');
			$('#divDescription').append('&lt;p&gt;&lt;b&gt;Author:&lt;/b&gt; ' + ui.item.author  + '&lt;/p&gt;');
			$('#divDescription').append('&lt;p&gt;&lt;b&gt;First published year:&lt;/b&gt; ' + ui.item.publishedDate  + '&lt;/p&gt;');			
			// and the usual description of the book
			$('#divDescription').append('&lt;p&gt;&lt;b&gt;Description:&lt;/b&gt; ' + ui.item.description  + '&lt;/p&gt;');
			// and show the link to oclc (if we have an isbn number)
			if (ui.item.isbn && ui.item.isbn[0].identifier)
			{
				$('#divDescription').append('&lt;P&gt;&lt;b&gt;ISBN:&lt;/b&gt; ' + ui.item.isbn[0].identifier + '&lt;/p&gt;');
				$('#divDescription').append('&lt;a href="http://www.worldcat.org/isbn/' + ui.item.isbn[0].identifier + '" target="_blank"&gt;View item on worldcat&lt;/a&gt;');
			}
        },
        minLength: 2 // set minimum length of text the user must enter
    });
});</code>
</pre>

An example text box is included on this page to try it out.