---
Title: wallpaper ebooks - how could it be done?
Description: an investigation into ebooks on walls - allowing users to access ebooks by holding their phone to wallpaper or posters
Type: Article
Tags: wallpaper ebooks,NFC tags,mobile,android,library technology
Template: blogpage
Author: dave
Date: 2014/07/01
---

On Twitter, [@publiclibnews](https://twitter.com/publiclibnews/status/476433830124199937) invited thoughts for the three best innovations all public libraries should adopt. One idea, pointed out by [@commonlibraries](https://twitter.com/commonlibraries) was this one (from 2012), of downloadable ebook posters in a subway station.

- [Digital Library on the Bucharest Subway](http://www.paulkater.com/2012/10/stunning-idea-digital-library-on-the-bucharest-subway-station/)

The idea certainly looks impressive, and is also very simple. QR codes are printed on the book spines on the posters, which provide links to free samples of ebooks hosted on a web site. To do something like that wouldn't be overly difficult or expensive. No greater cost than that of a custom poster or wallpaper, assuming there is a site available to serve the content.

A couple of limitations:

- Scanning QR codes generally needs to have some kind of app installed, to launch and then scan the code. This is possibly a bit of hassle for the end user, especially in a place like an underground station.
- It also relies on some kind of internet connection to get to the download site.

The first issue could be addressed using contactless technology. RFID and more recently NFC are established methods of transferring data to mobile devices, and are also on the increase. You hover the phone/device over the receiver/tag, it beeps (perhaps) and the device and receiver do whatever they need to do.

This could be the technology required to make an impressive ebook delivery platform. To start with, how to replace the existing idea with a contactless one?

#### With internet connection

If you assume the user has an active internet connection the problem is fairly simple: you use NFC tags instead of QR codes. A URL could be written to those tags by most NFC-capable devices, and those tags could be stuck directly on the poster. The user then holds their device up to the poster, it scans the tag, launches the URL and downloads the content.

- [Customised NFC stickers](http://rapidnfc.com/item/131/custom_print_nfc_stickers_44_x_55mm_rectangle_ntag203_express).  Around £1 each.

#### Without internet connection

Without an internet connection (such as at an underground station) it's trickier. Could the NFC tag itself transmit enough information for an ebook sample, without the need for holding the content elsewhere? No. Looking at [NFC tag memory sizes](http://rapidnfc.com/which_nfc_chip), the maximum amount of data is in bytes rather than kilobytes. Suitable for storing URLs and contact details, but not significant files.

To provide content in an area where internet access is unreliable you would need some kind of WiFi router, server, and digital content storage that didn't rely on an internet connection. You'd need to use something like a [LibraryBox](http://jasongriffey.net/librarybox/). These are $150 or build your own, and provide all those things - distributing digital content to connected users.

#### Automatic WiFi connection

With just a LibraryBox, posters, and some NFC tags, users could be given the option to hold their phone/tablet up to a poster and receive an ebook, even on the underground. But they would still need to be connected to the LibraryBox Wi-Fi. That's not a lot of trouble, but it removes the point of using NFC, which assumes you don't need to mess around in your phone apps or settings.

There are methods of using NFC tags to trigger tasks such as connecting to a WiFi network and then downloading content, but they require custom apps. They're useful for an individual - you could set up a tag at home which disables mobile data and connects your phone to your network when you arrive home. Or a tag on your car dashboard that turns on mobile data, GPS, and launches your navigation app. But very few users will have such an app installed, and it's not immediate if the user has to download and install an app first.  Such a user would more likely download a set of ebooks in advance to their device. The key is to draw in casual users who see the poster and want to use it there and then.

#### When?

In which case, the functionality to trigger the Wifi connection needs to be built into mobile operating systems.  This may be coming soon, Android 'L' (currently in preview) appears to be offering this functionality, allowing users to [write WiFi network connection details onto NFC tags](http://www.androidpolice.com/2014/06/27/android-l-feature-spotlight-write-wi-fi-passwords-to-nfc-tags-directly-from-android/). The preview is currently only available on two devices, the Google flagship Nexus 5 phone, and Nexus 7 tablet, but will start coming to most new devices later this year.

With the next iPhone rumoured to contain NFC capabilities, and Android enhancing their existing NFC, it may be possible that very soon ebooks and other content will be delivered without internet access direct to phones and tablets through posters, with just some stickers and without the need for any custom apps.