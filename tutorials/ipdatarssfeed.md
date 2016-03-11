/*
Title: use google apps scripts to make use of IP XML data
Description: creating an RSS feed from IP weekly lists.
Date: 2016/03/11
Type: Tutorial
TutorialComplexity: medium
TutorialSkillsRequired: JavaScript,scripting,XML
TutorialNeeded: a google account
TutorialTime: 2 hours
TutorialType: WebDevelopment
Template: tutorialpage
*/

**This is a post in preparation for an IP data hack event being held at Manchester Library on 12th March 2016.  The event is being held in a Google Garage so this uses Google Apps Scripts to query the published IP weekly XML data.**

## Google Apps Scripts

Google Apps Scripts can be created by users as part of their Google Drive documents.  The [Google Apps Script](https://developers.google.com/apps-script/) language is JavaScript with a number of additional methods available to simplify common scripting tasks, and integrate with other Google services. For example, you could write a Google apps script that queried your Google calendar and sent you customised alerts  (if you needed more than the built-in functionality).

Apps scripts can also be deployed as [Web Apps](https://developers.google.com/apps-script/guides/web).  These are designed to be shared to others as a URL and will serve content in to the user, this could either be HTML, or plain text.

## IP XML Weekly List

The Intellectual Property Office publish the Patent Journal as a weekly list, downloadable either in PDF or XML format.

- [IPO Patent Journal Weekly List](https://www.ipo.gov.uk/types/patent/p-os/p-journal/p-pj-download.htm)

The XML download is more useful than the PDF.  PDF is a useful format for reports designed to be read (and even then primarily in A4 printed format), but given that each week can amount to thousands of pages it seems unlikely that it will be read.   Any automated usage of the data would be easier done via the XML.

## RSS Feeds

An RSS Feed (Really Simply Syndication) is an XML format for web based content syndication.  There is also Atom, which is an alternative format.  Both are typically used as a way of following the publishing of content on websites, e.g. for news stories or blog posts.

- [RSS 2.0 Specification](http://cyber.law.harvard.edu/rss/rss.html)
- [Atom specification](http://www.atomenabled.org/developers/syndication/)

Regardless of these differing specs, most RSS readers will interpret RSS/Atom feeds that follow any standard specifications. 

## Tutorial

This tutorial will use a Google Apps Script to query the XML weekly list and convert it into a format that web users are more familiar with consuming - RSS.  The apps script will then be deployed as a web app, allowing it to be shared and accessed as a traditional RSS feed.

To create an apps script you need a Google account.

### 1. Create a new Google apps script file.

- In your browser, access Google Drive(https://drive.google.com)
- Select to create a new file by **New > More > Google Apps Script**.

To deploy the script as a webapp it needs to have a method called doGet().  Modify the file to look like the following:

<pre class="prettyprint linenums">
<code>function doGet() {
}</code>
</pre>

The code for the script will all be put within the doGet function.

### 2. Accessing the patent data XML

A refreshed XML file is created each week by the IPO and is accessible from a direct URL such as:

[https://www.ipo.gov.uk/p-pj-fullfile/xml/6616/6616.xml](https://www.ipo.gov.uk/p-pj-fullfile/xml/6616/6616.xml)

Each week the journal will have a new number (in the case above this is **6616**).

Any code trying to access the latest list needs to calculate what the current list number is.  For this to be done it needs a reference value.  If we take 6616 as being from Wednesday 10th March, then for any future dates we just need to plus 1 to that number for every full week passed.

When the code has the current Journal ID it can then access the data using the built in method **UrlFetchApp.fetch**.

<pre class="prettyprint linenums">
<code>// Calculate the ID of the journal we're currently on
var referenceDate = new Date(2016,03,09);
var currentDate = new Date();
// The difference between the dates is returned in milliseconds.  Convert that to weeks (/604800000)
var weeks = Math.floor((currentDate-referenceDate) / 604800000);
var journalId = 6616 + weeks;
var latestJournalText = UrlFetchApp.fetch('https://www.ipo.gov.uk/p-pj-fullfile/xml/' + journalId + '/' + journalId +'.xml').getContentText();</code>
</pre>

### 3. Read the data as XML

The XML is split into a number of headings and sections.  The headings are: 

- Proceedings under the Patents Act 1977
- Other Proceedings under the Patents Act 1977
- Supplementary Protection Certificates.

Each of these headings is split into sections summarised in the following table (note: this table is not essential reading)

<table>
<tr><th>Heading</th><th>Section</th></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>Applications for Patents filed</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>Applications Terminated before Publication under Section 16(1)</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>Applications Terminated after Publication under Section 16(1)</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>Applications Published</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>Patents Granted</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>European Patents Granted</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>Translations Filed</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>European Patents Revoked</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>European Patents Expired</td></tr>
<tr><td>Proceedings under the Patents Act 1977</td><td>UK Patents Expired</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Reinstatement of Applications</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Amendment of specification after grant</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Restoration of ceased patents</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Surrender of patent</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Transactions in Patents and Applications</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Register of patents</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Claim/Counterclaim for Revocation Before the Court</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Revocation on Comptroller's Initiative</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Amendment of patent in infringement or revocation proceedings before the Court.</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Amendment of patent in infringement or revocation proceedings before the comptroller</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Request for Publication of Translation</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Correction of errors in Patents and Applications</td></tr>
<tr><td>Other Proceedings under the Patents Act 1977</td><td>Appeals to the Court</td></tr>
<tr><td>Supplementary Protection Certificates</td><td>SPCs Lodged</td></tr>
<tr><td>Supplementary Protection Certificates</td><td>SPCs Granted</td></tr>
<tr><td>Supplementary Protection Certificates</td><td>SPCs Lapsed</td></tr>
</table>

A sample of the XML structure is:

<pre class="prettyprint linenums">
<code>&lt;Journal Number="6616" Date="9 March 2016" ApplicationNoStart="GB1601103.3" ApplicationNoEnd="GB1601537.2" PublicationNoStart="GB2529798" PublicationNoEnd="GB2529995"&gt;
	&lt;Headings&gt;
		&lt;Heading Name="Proceedings under the Patents Act 1977" InternalName="Main" ID="1"&gt;
			&lt;Sections&gt;
				&lt;Section Title="Applications for Patents filed" InternalName="ApplicationsFiled" ID="1"&gt;
					&lt;Case&gt;
						&lt;CaseID&gt;1822232&lt;/CaseID&gt;
						&lt;ApplicationNo&gt;GB1601394.8&lt;/ApplicationNo&gt;
						&lt;Title&gt;Seat belt adjuster&lt;/Title&gt;
						&lt;CaseStatus&gt;AWAITING FORM 9&lt;/CaseStatus&gt;
						&lt;DateLodged&gt;23 January 2016&lt;/DateLodged&gt;
						&lt;DateFiled&gt;23 January 2016&lt;/DateFiled&gt;
						&lt;NotEnglish&gt;false&lt;/NotEnglish&gt;
					&lt;/Case&gt;
					...More cases...
				&lt;/Section&gt;
			&lt;/Sections
			...More sections...
		&lt;/Heading&gt;
		...More headings...
	&lt;/Headings&gt;
&lt;/Journal&gt;</code>
</pre>

Putting all the data into a single RSS field would be a little too unwieldy, and have little focus given the scope of what is covered in that XML.  To ask a more specific question of the data, this particular feed will list cases from the Patents Granted section.  It would of course be possible to create many instances of the script to query different aspects of the data, or Google Apps scripts also allow for passing in parameters as part of the URL which could specify sections to return, and even keywords to search by.

Back in the script, fetching the URL will just fetch the raw text data.  For the script to actually understand the data as XML and to extract values from it, it needs to be explicitly loaded as XML.  This is done using the **XmlService.parse** method.  Once that is done there are various methods to select specific sections of the data from the XML using the **getChild()** and **getChildren()** XML methods, starting at the root (**getRootElement()**).

<pre class="prettyprint linenums">
<code>// Parse the XML to get a list from the applications filed section.
var journalXml = XmlService.parse(latestJournalText);
var sections = journalXml.getRootElement().getChild('Headings').getChild('Heading').getChild('Sections').getChildren('Section');
// We are selecting the Patents Granted section which is index 4
var patentsGranted = sections[4].getChild('Cases').getChildren('Case');</code>
</pre>

### 4. Create the RSS Feed

There isn't a lot of detail provided in the XML for each Patent. The script will build an RSS 2.0 compliant feed, using the following available fields from the XML for each **item** element in the resulting RSS.

- **GrantTitle** - The title of the the patent, will map to the **title** mandatory element in the RSS.  It will also be used in the **description** element.
- **PublicationNo** - The publication number, will be used to create the  **link** element in the RSS and the **guid** element (a unique identifier).
- **CaseStatus** - The case status, will be used for the **category** element in the RSS.

The code in the previous step provided a list of Patents Granted that the script can loop through.  To create the necessary RSS data the script again uses the **XmlService** (RSS being XML based).  At the end it returns the content using the  **ContentService.createTextOutput** method, which is necessary when deploying as a Web App.

<pre class="prettyprint linenums">
<code>// Create the RSS document
var root = XmlService.createElement('rss');
var channel = XmlService.createElement('channel')
	.addContent(XmlService.createElement('title').setText('Proceedings under the Patents Act 1977'))
	.addContent(XmlService.createElement('description').setText('Proceedings under the Patents Act 1977 - Patents Granted'))
	.addContent(XmlService.createElement('language').setText('en-GB'));
var rssDocument = XmlService.createDocument(root);
  
// For each one of the cases, add in an RSS entry.
for (var j = 0; j < applicationsFiled.length; j++) {
	var item = XmlService.createElement('item')
		.addContent(XmlService.createElement('title').setText(applicationsFiled[j].getChild('GrantTitle').getText()))
		.addContent(XmlService.createElement('link').setText('https://www.ipo.gov.uk/p-ipsum/Case/PublicationNumber/' + applicationsFiled[j].getChild('PublicationNo').getText()))
		.addContent(XmlService.createElement('category').setText(applicationsFiled[j].getChild('CaseStatus').getText()))
		.addContent(XmlService.createElement('guid').setText(applicationsFiled[j].getChild('CaseID').getText()));
  	channel.addContent(item);
}
root.addContent(channel);
var rss = XmlService.getPrettyFormat().format(rssDocument);
return ContentService.createTextOutput(rss).setMimeType(ContentService.MimeType.RSS);</code>
</pre>

### 5. Deploy as web app.

Within the apps script screen the Publish menu has an option to publish: **Publish > Deploy as web app**

The option will present a screen with some options:

- **App URL** - Automatically generated.
- **Project version** - *New* - also gives an opportunity to give the version a description.
- **Execute as** - Can be your Google account, or the user accessing the web app.  Primarily more used when writing apps that use other Google service (e.g. that integrate with the user's calendar).
- **Access** - Can be restricted (which would require a Google account) or *anyone*.

An example published URL is available at:

[IP Patents Jounal - Patents Granted RSS](https://script.google.com/macros/s/AKfycbxkCkH7g4sbq6v0E3w-C8h9k1znobdrOjXl1O142YF0Ta0Gh2cP/exec)
