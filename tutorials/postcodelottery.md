---
Title: library postcode lottery
Description: play the english library postcode lottery
Type: Tutorial
Tags: postcodes, library locations, public libraries, england
Template: tutorialpage
Author: dave
Date: 2018/01/27
GitHub:
TutorialType: GIS
TutorialComplexity: medium
TutorialSkillsRequired: SQL
TutorialNeeded: PostgreSQL database
TutorialTime: 1 hour
---

Put in your postcode and see your england library postcode lottery result.

The possible grades are A*, A, B, C, D, E, F. 

<div id="div-lottery"></div>

If your postcode doesn't seem to work then maybe it doesn't really exist?

#### methodology

What's a postcode lottery? Google says:

>> a situation in which someone's access to health services or medical treatment is determined by the area of the country in which they live 

What's a library postcode lottery? Google say:

>> a situation in which someone's access to libraries is determined by the area of the country in which they live

(Google don't actually say this but once this article is indexed, they may do, it's in the metadata now)

Ages ago in 2016 the England Libraries Taskforce released a dataset of all the locations (postcodes) of libraries in England. Postcodes and libraries, the possibilities are endless. But the most obvious one is grading every postcode location in England on how close it is to a library.

It's very simple straight line distances. No clever driving or walking distances involved. If there's a river in the way the tool assumes you swim it in the most direct line possible.

But not all areas in England are comparable in terms of 'distance to a library'. Country folk know they don't live next to things, and don't expect to. When I was young the nearest library was either 8 miles away, or in the holidays a 1.5 mile cycle to the weekly mobile library stop. What was lost in proximity to libraries was gained in other things. Like the local cider farm, which was very generous about tasting. That was a similar cycling distance to the mobile stop, so for a good round trip you could cycle to the cider farm, put a gallon of cider in your bag, cycle to the mobile library to get some books, and have a good evening.

The point is, we can't be feeling too sorry for rural areas on all these postcode lotteries. So, let's throw in a measure of 'rurality' and rate rural and urban postcodes separately. What are rural areas? Gov.uk go for the perfect definition:

>> "Rural areas are those areas that are not urban"



So, what are urban areas?

"Urban areas are those areas that are not rural"
- Not really


We do need actual thresholds to define rural/urban. The 'rural urban classification'(https://www.gov.uk/government/statistics/defining-rural-areas) has much to say on this, and also mentions  a census output areas classification of rural/urban. Census output areas are small areas used in the production of statistics (such as from the census). The definition was that https://www.ons.gov.uk/methodology/geography/geographicalproducts/ruralurbanclassifications/2011ruralurbanclassification

Great, because we can download the census output areas. And we can download the census output area rural urban classifications (https://ons.maps.arcgis.com/home/item.html?id=3ce248e9651f4dc094f84a4c5de18655). And we can match postcodes to census output areas. Therefore we can match postcodes to rural urban classifications.


##### ranking

We don't want to show 'you are ranked 

Note: Zoopla and RightMove have been alerted to the existence of this and will probably be integrating the data into their house assessments in the near future.
