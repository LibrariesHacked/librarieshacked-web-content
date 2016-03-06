/*
Title: NHS choices
Description: an api for local health data (nearest gp, dentist...)
Type: PublicInfoApi
ApiKey: key required. register for access.
ApiExampleText: microsoft health vault
ApiExampleUrl: http://www.nhs.uk/aboutNHSChoices/professionals/syndication/Pages/microsoft-case-study.aspx
ApiUrl: http://www.nhs.uk/aboutNHSChoices/professionals/syndication/Pages/Webservices.aspx
Format: XML
ApiDisplayPage: true
*/

although not an API directly dealing with library data, in 2013 the SCL (society of chief librarians) announced the 4 *universal offers*, '*the four key areas of service which our customers and our stakeholders see as being integral to a 21st century library service.*'  these were digital, information, reading, and health. so with that in mind, this API is one that would be very appropriate to mix in during a library hack.

## what data is available?
there is a lot of NHS based data available from this API.  this includes:

- local nhs services to a location
- news
- health a-z
- and more

## how to retrieve data

data is called using the RESTful based API, making it easy to construct a URL into a query.  a typical request may be:

- http://v1.syndication.nhschoices.nhs.uk/livewell/topics/focusonchlamydia?apikey=APIKEY

(where API key is the key you'll be sent on registration)

## return data format

the data is returned in XML format.

## usage conditions

to use the API you need a key.  follow the instructions on the website to get this, it will consist of various questions about who you are and why you wish to use the data.  the documentation also includes conditions of use, so pay attention to those.
