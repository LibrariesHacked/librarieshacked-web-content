---
Title: connecting and opening local government data
Description: the challenges of opening up data from local government systems
Type: Article
Date: 2014/10/07
Author: @librarieshacked
Template: blogpage
Tags: linked data, open government data, government software
---

Open data is widely considered to be a **good thing** - greater organisation and government transparency leads to a better informed public and greater innovation and creative use of that data.

The government promote an open data policy.  The service design manual on gov.uk includes [a page on open data](https://www.gov.uk/service-manual/technology/open-data.html).  Key phrases:

- *You should publish all public data, unless it is private data collected from people or restricted for national security reasons.*
- *Your open data should be user-friendly and findable. You should support people who want to reuse it and provide guarantees about how it will be made available.*
- *Public bodies should actively encourage the reuse of their public data.*

... and many more such examples.  It reads like a manifesto in support of open data, and will be gratifying for those who are keen for more public data to be published in reusable formats.

Despite all this, for a local council the question will still be *why?* and though arguments for open data are compelling, they are rarely ones that provide a measurable cost saving, yet they often present implementation costs.

## what data does a council have?
Councils are not typically organisations that are on top of the data they hold.  For example, they may have the following systems:

- an electoral registration database for those registered to vote.
- a library member database.
- a database of planning and licensing applications with associated data on individuals (those who have made applications and those who have commented on applications).
- a CRM (customer relationship management) database used by the customer services department to keep track of engagement with residents.
- a *revenues and benefits* system with personal details of those paying/receiving money from the council (e.g. council tax/housing benefit).
- various GIS (geographic information systems) databases such as school locations, air quality data, or waste/recycling collection routes.
- social care systems, school admissions, and countless other and smaller systems such as allotment holdings and waiting lists.

If a council is going to spend money on opening data to the public they will need to: 

- assess what can be made available
- create a platform to publish that data 
- anonymise and aggregate data where appropriate
- query different systems and platforms to extract the data.
- keep those exports up to date

Despite holding this wealth of information, the systems will rarely be *linked together* in any way, despite all the data being related.  Those registered to vote will also be on the council tax and benefits systems, and the CRM system, and the library system.  Their house will be a registered property which will be on waste and recycling collection routes.  They will be a member of a council ward, which will have specific health data, which could be linked to relevant air quality monitors, or directly linked to council spend in that area.

## the cost of opening up data
Unfortunately, any question asked of a council that involves data from more than one system will likely prove a challenge (and be expensive) to answer - because those systems will be made by a variety of suppliers with a mix of technologies and database systems (sql server, oracle, access, *excel!*), and even hosted on different operating systems.

Those suppliers will also rarely provide the means of exporting and exposing data in standard formats, other than through specific work requests.  Yet [this article from computer weekly](http://www.computerweekly.com/news/2240082024/Local-government-software-market-dominated-by-six-firms) suggests that local government software is dominated by just 6 firms.  So why aren't there better standards and processes for connecting those systems?

It may be because it is not in those suppliers interests to provide solutions that allow for easy export/import of data.  In most cases software procurement for local government has already been done, and it was done at a time when there was little focus on open data.  Future income for suppliers needs to come from two sources: licensing costs, and service requests.

Ongoing licensing income requires that councils keep those systems in place - not guaranteed if the data held can easily be exported to a competitor.  Service requests can often come from requirements to provide small integrations with other systems, or provide ad-hoc data extraction.  Again, not a revenue stream that will remain if the system does those things *out of the box*.

This has made for two classes of public sector software suppliers.  Suppliers that advertise modern solutions that include open data and interconnectivity as a selling point: these struggle to get into the market as they need to replace mission critical systems.  And those suppliers that are already embedded within government: they need to ensure they remain so, and so prohibit the possibility of being replaced.  This has effectively led to a stalemate in local government software provision, putting open data aspirations on hold.

## breaking the stalemate
A culture of open data doesn't just mean exposing as much data as possible to the public, it requires a complete change of approach with regards to use and procurement of software.  To really promote an open data culture, the government would need to enforce rules on council suppliers: to prohibit the use of any systems that do not provide exports of data in useful formats, as well as ensuring they provide APIs (application programming interfaces) for interconnectivity with the data they hold.

This is not outside the realms of possibility.  The aspirations of the government service design manual are currently at odds with the reality in local government software, and this should become increasingly clear as open data objectives are not met and demand for data increases.

The difficulties of opening up data for councils are such that they currently require monetary incentives to do so.  The Local Government Association recently provided [an incentive scheme](http://incentive.opendata.esd.org.uk/) for councils to open up three datasets: planning applications, premises licenses, and public toilets.  The idea is that if all councils publish these in a standard format, they can be aggregated together (a find my toilet app that finds the user's nearest public toilet?).  But without the monetary incentive of £2000 per dataset, with an extra £1000 for doing all 3, it is unlikely that many councils would find the time to arrange such exports that should be part of routine process.

## real benefits
None of this quite answers the question of why open data can be of cost benefit to a council.  And it is unlikely that releasing open data itself will provide immediate savings.  Short-term it is likely to be an expensive exercise, including clearing out suppliers that do not support goals for open data, and dealing with the pain of migrating from outdated software systems.

Long term savings then do become clearer as those license fees drop in a more open and competitive market (or open source alternatives can be adopted), and service requests reduce as small simple tasks can be done internally.  Then the benefit of having data connected will allow councils to make better use of that data.  Even council tax fraud will be a lot easier to detect if data is interlinked between systems (the library database system shows two active residents at an address while the council tax system shows a single person discount still in place).  More positive examples will be being able to open up data to developer communities to work on useful civic hacks.

Until the change in software systems happens, the basis of open government data will largely be small, ad-hoc, time consuming, and expensive exports of data.  Although it is a bigger task, if the software is taken as a starting point then the data will follow.