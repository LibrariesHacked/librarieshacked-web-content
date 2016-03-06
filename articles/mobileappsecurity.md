/*
Title: mobile app security testing
Description: investigating the security of mobile apps.
Type: Article
Date: 2015/05/31
Author: dave
Template: blogpage
Tags: mobile apps, library apps, HTTPS, security, personal data
*/

###web app security and architecture
Web browsers provide a set of information to ensure users are kept informed of online security.  The user can see if a connection is encrypted (HTTPS, generally indicated by a padlock symbol), and if so the browser will ensure various standards, such as that all individual requests on a page use encryption (protecting against [mixed content](https://developer.mozilla.org/en-US/docs/Security/MixedContent)), and that the [encryption certificate](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/) is valid.

Web apps also traditionally only expose one application *tier* to the user.  An app can be split into three tiers:

- presentation (what the user sees and interacts with)
- logic (saving and retrieving from storage, external services, executing commands)
- data (user details, application data).  

The user interacts with the web pages (presentation tier) in the browser, the logic and data are then largely handled by the web server and database server, not exposed to the public.

###mobile apps
Mobile apps are a significant change in architecture.  Splitting by presentation/logic/data again, the presentation tier is still displayed to the user on their device, but the device will also hold much of the logic and the data tier (assuming the app allows for storing of data such as user credentials).

From a security point of view both the logic and data tiers are then more vulnerable than they were within the web architecture, as they are being held on user devices.  The connection from the app and the web is through web services, which need to be encrypted, and the data held on the app needs to also be held securely.

Despite this, mobile operating systems give less control and information than browsers over security standards.  A mobile app can access web services over a mix of encrypted and unencrypted connections, with the user being uninformed.  Similarly, data stored on the user's device can be held in whatever way the developers wish (secure or not).

A *hybrid* mobile app is one where the app acts like a web server on the device, displaying web pages that are held locally within the app.  All a developer needs to do is write those pages, and make small modifications to an out of the box *base* app.  It is a good short-cut to writing individual *native* apps, as the web pages can be deployed into base apps that are written for all app stores.  The downside is the result tends to look more *web-like* and less integrated with the native operating system.  One of the most popular hybrid app frameworks is [Apache Cordova](http://cordova.apache.org/).  But despite using web technologies, hybrid apps still have the same security concerns as native ones.  A notable [security alert](http://cordova.apache.org/announcements/2014/08/04/android-351.html) was sent by Google to all apps on the Play Store that were built with a version of Cordova below 3.5.1; the security vulnerability being that the app could be launched to load HTML not originally part of the app, which could also then reveal app data (e.g. user credentials).

###analysing app security
Without significant built-in security features on mobile operating systems, analysis of an app is largely left down to the user.  There are tools and options for achieving this:

- On Android there are a number of apps that will record network activity during a period of time.  Encrypted data will be unrecognisable, unencrypted data will be easy to identify.  The best way to find these is to search the relevant app stores for 'packet capture' tools.
- A more real-life 'hacker test' could be to actually monitor network traffic from another computer, while connected to the same network as the mobile device.  This generally requires a non-Windows PC (or an [AirPcap adapter for Windows](https://wiki.wireshark.org/CaptureSetup/WLAN#AirPcap)), and a tool like [Wireshark](https://www.wireshark.org/).
- There are simulators for running iOS and Android apps on Windows and OSX environments - allowing for tracing of the data sent from the PC using established PC tools (like WireShark above).
- Analyse the code.  For Android, most apps installed are available to analyse by *decompiling*, which will often reveal information such as which URLs are being accessed.  For Android, the installed app can be extracted from the device to a single file (an *APK* file) and then a tool like [APKtool ](http://ibotpeaches.github.io/Apktool/) used to extract the contents.  There are also apps that run on the mobile device to interactively decode installed apps [such as Dexplorer](https://play.google.com/store/apps/details?id=com.dexplorer). Or there is [Apk2Java](http://www.apk2java.com/), an online service that will allow upload of an APK file and then list the code contents, and all URLs in the app.

*Monitoring network traffic should always be done on private (with permission), not public networks!*

Using a variety of these tools, this post looks at a number of publicly available library membership apps to determine the security of the web services used to interact with the library catalogue and member data.

###solus
app type: hybrid

example: [Manchester libraries](https://play.google.com/store/apps/details?id=uk.co.solus.manchesterlibraries)

Solus supply many UK authorities with a library membership app.  The app is built using [PhoneGap](http://phonegap.com/)/[Apache Cordova](http://cordova.apache.org/), version 2.7.0.

Solus release an app per authority, though the underlying app and HTML/JavaScript files remain largely the same.  The primary change being the authority ID passed to a set of web services during various data requests (login, loans, renew, etc).  Solus choose to access each individual authority from behind the security of their own infrastructure, exposing publicly just their own web service.  From a design point of view this is clean and allows them to access a variety of different types of library technology suppliers without putting all that logic into the app, or giving away details of those web connections to individual authorities.

The mobile *endpoint* for these web services seems to be an HTTP one *(correction: see note below)*, rather than being HTTPS, so from network traces it appears data such as account number and PIN are sent unencrypted.  This isn't really a rarity with mobile apps, but it does highlight the difference between established standards in web and mobile app security.

*note (25-11-2015): checking this again it appears the Solus mobile endpoint is now HTTPS, with a newly installed certificate.  The Cordova version has also been upgraded (and when decoding, the app code has been minified/obfuscated)*

###axiell
app type: native

example: [axiell my library](https://play.google.com/store/apps/details?id=dk.bridgeit.axiell.mylibrary&hl=en)

Axiell are a library technology supplier providing solutions such as *OpenGalaxy*.  The Axiell My Library app takes a different approach to the Solus one.  Rather than provide an app for each authority, the app lists all participating authorities and the user chooses their own.

The Axiell app also differs from the Solus one in that it does not use a fixed web service, it directly accesses each authority by retrieving data from a direct URL.  This brings more of the logic into the app itself, but also makes the process more efficient - an intermediate web service creates an extra step to retrieving data.

Because the Axiell mobile app uses individual web endpoints for each library service, the answer to whether data is transmitted securely is *it depends*.  Some authorities have web services which are HTTPS, others use just HTTP - this makes it a slight gamble for the user as to the level of security they are provided with.

###sirsi dynix
app type: hybrid.

example: [BookMyne](https://play.google.com/store/apps/details?id=sirsidynix.bookmyne) (e.g. Sutton Council)

Sirsi Dynix also have a single app to access library services across multiple library services.  Any library service that have [Sirsi Dynix web services](http://www.sirsidynix.com/products/api-web-services) installed can make use of the BookMyne mobile application.

In this case the app is also hybrid, displaying web pages rather than building a native UI.  There is a slight difference though - the web pages themselves are retrieved and saved locally from the Internet, at an HTTPS address.  An approach that means the code of the app can effectively be changed without upgrading the app itself.

BookMyne use a set of web services under that same HTTPS address, so it appears do encrypt the data passed back and forth.

###civica
app type: native

example: [sorcer](https://play.google.com/store/apps/details?id=com.civica.sorcer) (e.g. Lincolnshire libraries)

Civica provide Sorcer mobile for users to access their library account.  Another single library app, this integrates with authorities who are using [Spydus](https://www.civica.co.uk/library-and-learning/spydus_opac).

The app is written in native code (e.g. Java for the Android version).  Like the Axiell app, it directly accesses different URLs for each registered library.  Again, these appear to be a mix of HTTP and HTTPS, so the security of the app is dependent on which service the user is member of.

###future developments
The current state of mobile app security standards (despite apps being in development for quite a few years) can be said to be immature compared with web security.  Issues for users such as unencrypted web services, or allowing encryption for only some users, highlight why leaving security concerns up to the good will of suppliers may not be a long term option.

As browser competition has pushed up web security standards, operating system providers (Android/iOS/Windows) need to provide similar standards for their users - alerting users when any web services are being accessed that are not encrypted, and making unencrypted traffic an explicit permission that the app needs to request.