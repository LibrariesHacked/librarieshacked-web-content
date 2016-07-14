---
Title: a single view of library services
Description: portals to interact with and combine library catalogue services across authorities and sectors.
Type: Article
Date: 2015/01/21
Author: @librarieshacked
Template: blogpagecatalogue
Tags: library catalogues, web services, library management systems
---

There is a mobile app on the Google Play store that combines many library services into one for users to view loans, receive notifications, and renew items - across all their library accounts.

[Library Reminder - Google Play store](https://play.google.com/store/apps/details?id=uk.co.fk33.lr.uk.free)

## web scraping/automation

The app currently covers around 300 UK college, health, university, and public libraries.  Library systems do offer integration options that are used by third parties, but these generally aren't available to public developers. In this case the integration has been developed purely from automating publicly available web functions (using code to mimic the actions of a user on a website).

It's possible for the app to provide integrations for so many libraries because there aren't that many types of web catalogues.  With a list of libraries, which portal they use, and code to interact with the different types of portal, code is reused across multiple services.  Not a bad prospect if a user is member of a number of different libraries and wants somewhere to manage all their accounts.  Just enter the library credentials and let the app access the different sites.  The app just needs to know the catalogue type, and URL.

| library | catalogue type | link |
| ------- | -------------- | ---- |
| the college of optometrists | heritage | http://library.college-optometrists.org/HeritageScripts/Hapi.dll |
| bath university | heritage | http://library.bath.ac.uk/uhtbin/cgisirsi/X/X/X/29/X/X/3// |
| oxfordshire | viewpoint | https://www.libcat.oxfordshire.gov.uk/ |

However, entering membership IDs and passwords into unofficial apps may not be too appealing for users.  There's no guarantee the developer won't use those usernames and passwords for their own purposes, or that the credentials are stored/transmitted securely.  (This latter point is not guaranteed in official apps either, but there's an expectation of accountability).

The [Sieghart report](https://www.gov.uk/government/publications/independent-library-report-for-england) mentions a digital network that *'could include a single library platform and a national library card and catalogue'*.  Wales tendered for a single library system, but given the cost ('*Range between 1000000 and 3000000 Currency: GBP*'), it may be a while before seeing this across England.

## web services

What these apps, and other examples make clear is that underlying systems don't need to be the same to operate together, or to merge into a single service.  Web services can provide the illusion of a single system, without necessarily procuring for one.  Effectively then building a discovery layer (of which many already exist) that unifies the library service experience for users.

[Solus - your library app](http://www.yourlibraryapp.com/postcards.html) integrates against various public libraries (over 70) to provide a single web service to provide library functions, but then re-creates the separation by having each authority market their own app.  [The Axiell My library app](https://play.google.com/store/apps/details?id=dk.bridgeit.axiell.mylibrary&hl=en_GB) does the opposite - it provides multi-library service functionality from a single app, but users can only register one set of membership details at a time.  Similarly, other suppliers apps such as [BookMyne](http://www.sirsidynix.com/products/bookmyne) (SirsiDynix) and [Sorcer](http://civicalld.com/news/sorcer-mobile-now-on-android) (Civica) provide a web service layer that provides access into many libraries.

A merging together of all the above services could very quickly pull in enough libraries to create a single app/website to manage a library user's account(s).

Private web services also don't always lead to the best result for users.  As the recent [moonpig API flaw demonstrated](http://www.programmableweb.com/news/moonpig-api-flaw-exposes-data-over-3-million-customers/2015/01/06), which exposed personal data of millions of customers, private APIs are only private until they are discovered.

## open web services and data

It would be interesting if England attempted to not develop a single LMS, but a set of open web services to provide public catalogue and membership services - integrating with existing systems rather than replacing them.  Given the choice of free software people have for accessing various other accounts (Twitter for example) - within a short space of time there could be a real variety of systems (including open-source) that best serve user convenience.

As soon as systems are made open, usage potential is enhanced.  GoodReads users can embed a widget on webpages to show what they're currently reading - the same could be available for library loans.

When considering whether such open web services present a security issue, a recent [ProgrammableWeb article on private APIs](http://www.programmableweb.com/news/how-hackers-crack-supposedly-secure-and-private-apis/analysis/2015/01/12) included the notable point: *'APIs, like crypto implementations, are at their best when they’re developed openly and widely scrutinized'*.