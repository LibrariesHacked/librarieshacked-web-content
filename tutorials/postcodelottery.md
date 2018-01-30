---
Title: library postcode lottery
Description: play the English library postcode lottery
Type: Tutorial
Tags: postcodes, library locations, public libraries, england
Template: tutorialpagepostcodelottery
Author: dave
Date: 2018/01/30
GitHub: tutorial-postcodelottery
TutorialType: GIS
TutorialComplexity: medium
TutorialSkillsRequired: SQL
TutorialNeeded: PostgreSQL database
TutorialTime: 2 hours
---
If your postcode doesn't seem to work, maybe it doesn't really exist?

#### Methodology

What's a postcode lottery? Google says:

> a situation in which someone's access to health services or medical treatment is determined by the area of the country in which they live 

What's a library postcode lottery? Google says:

> a situation in which someone's access to libraries is determined by the area of the country in which they live

(Google don't actually say this but once this is indexed, they may do, it's in the metadata now)

Ages ago, in 2017, the England Libraries Taskforce [released a dataset](https://www.gov.uk/government/publications/public-libraries-in-england-basic-dataset) of all the locations (postcodes) of libraries in England. Postcodes and libraries, the possibilities are endless. But the most obvious one is grading every postcode in England on how close it is to a library.

This is done in very simple straight line distances. No clever driving or walking distances involved. If there's a river in the way, you swim it in a direct line to the library.

But regardless of those flaws, not all areas in England are comparable. Country folk know they don't live next to things, and don't expect to. When I was young the nearest library was either 8 miles away, or in the holidays a 1.5 mile cycle to the weekly mobile library stop. What was lost in proximity to libraries was gained in other things. Like the local cider farm, which was very generous about tasting. That was a similar cycling distance to the mobile stop, so for a good round trip you could cycle to the cider farm, put a gallon of cider in your rucksack, cycle to the mobile library to get some books, and have a good evening.

The point is, let's throw in a measure of 'rurality', and rate rural/urban postcodes separately. 

What are rural areas? [This PDF from Gov.uk](https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/591462/RUCOA_leaflet_Jan2017.pdf) goes for the perfect definition:

> Rural areas are those areas that are not urban

So, what are urban areas?

> Urban areas are those areas that are not rural

We do need actual thresholds to define rural/urban. The [rural urban classification](https://www.gov.uk/government/statistics/defining-rural-areas) has much to say on this, and defines urban areas as settlements of over 10,000 people. It goes further and splits the classification into 10 groups.

- A1 Urban major conurbation
- B1 Urban minor conurbation
- C1 Urban city and town
- C2 Urban city and town in a sparse setting
- D1 Rural town and fringe
- D2 Rural town and fringe in a sparse setting
- E1 Rural village
- E2 Rural village in a sparse setting
- F1 Rural hamlets and isolated dwellings
- F2 Rural hamlets and isolated dwellings in a sparse setting

The Office for National Statistics don't provide this classification per postcode but [do at census output area](https://ons.maps.arcgis.com/home/item.html?id=3ce248e9651f4dc094f84a4c5de18655) level. Census output areas are small areas used in the production of statistics.

Great, because we can also download [the census output boundaries](http://geoportal.statistics.gov.uk/datasets/output-area-december-2011-full-clipped-boundaries-in-england-and-wales). And we can download all the [postcode centres from Ordnance Survey](https://www.ordnancesurvey.co.uk/business-and-government/products/code-point-open.html) to match postcodes to census output areas. Therefore we can match postcodes to rural urban classifications.

That allows us to calculate the distance from each postcode centre to the nearest library. Then rank these, grouped by the rural urban classification.