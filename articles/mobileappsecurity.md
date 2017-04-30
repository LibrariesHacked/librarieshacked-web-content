---
Title: mobile app security testing
Description: investigating the security of mobile apps.
Date: 2015/05/31
Type: Article
Author: dave
Tags: mobile apps,library apps,HTTPS,security,personal data
Template: blogpage
---

Web app security and architecture
---------------------------------

Web browsers provide a set of information to ensure users are kept informed of online security.  The user can see if a connection is encrypted.  The web address will be HTTPS (rather than HTTP), and there will be an indication of security such as a padlock symbol and a green coloured address bar.  When using encrypted connections the browser will also ensure various standards to make sure that security isn't compromised. This includes ensuring that all requests on the page use encryption (protecting against [mixed content](https://developer.mozilla.org/en-US/docs/Security/MixedContent)), and that the [encryption certificate](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/) is valid.

Web sites only expose one application tier to the user.  Spliting a web application into three tiers:

- presentation (what the user sees and interacts with)
- logic (saving and retrieving from storage, external services, executing commands)
- data (user details, application data).

The user interacts with the web pages (presentation tier) in the browser, the logic and data are then largely handled by the web server and database server, not exposed to the public (aside from selected data that is displayed on those pages).

Mobile apps
-----------

Mobile apps are a significant change in architecture.  Splitting by presentation/logic/data again, the presentation tier is still displayed to the user on their device, but the device will also hold more of the logic and the data tier.  This exposes more of the process used to construct the presentation.  From a security point of view both the logic and data tiers are then more vulnerable.

Despite this, mobile operating systems give less information that browsers over security standards.  A mobile app can access web services over a mix of encrypted and unencrypted connections, with the user being uninformed.  Similarly, data stored on the user's device can be held in whatever way the developers wish (encrypted or not).

A hybrid mobile app is one where the app acts as a cointainer for webpages held on the device.  All a developer needs to do is write those pages, and make small modifications to the container app to customise it (app icon, etc).  It is a good short-cut to writing individual *native* apps, and means that a single set of code can be deployed to all app stores.  One of the most popular hybrid app frameworks is [Apache Cordova](http://cordova.apache.org/).  The downside is the result tends to look more web-like and less integrated with the native operating system.

Analysing app security
----------------------

Without built-in security features on mobile operating systems, security analysis of an app is largely left down to the user.  There are tools and options for achieving this:

- On Android many apps will record network activity during a period of time.  Encrypted data will be unrecognisable, unencrypted data will be easy to identify.  The best way to find these is to search the relevant app stores for 'packet capture' tools (and if using one try to ensure it appears reputable!)
- A more real-life 'hacker test' could be to actually monitor network traffic, while on a PC connected to the same network as the mobile device.  This generally requires a non-Windows PC (or an [AirPcap adapter for Windows](https://wiki.wireshark.org/CaptureSetup/WLAN#AirPcap)), and a tool like [Wireshark](https://www.wireshark.org/).
- There are simulators for running iOS and Android apps on Windows and OSX environments - allowing for tracing of the data sent from the PC using established PC tools (like WireShark above).
- Analyse the code.  For Android, most apps installed are available to analyse by a process called decompiling.  This will reveal information such as which URLs are being accessed.  For Android, the installed app can be extracted from the device to a single file (an APK file).  A tool such as [APKtool](http://ibotpeaches.github.io/Apktool/) can then extract the contents.  There are also apps that run on the mobile device to interactively decode installed apps [such as Dexplorer](https://play.google.com/store/apps/details?id=com.dexplorer). Or there is [Apk2Java](http://www.apk2java.com/), an online service that will allow upload of an APK file and then list the code contents, and any URLs in the app.

**Monitoring network traffic should always be done on private, not public networks (with permission from anyone being monitored)!**

Using a variety of these tools, this post looks at a number of publicly available library membership apps to determine the security of the web services used to interact with the library catalogue and member data.

Solus
-----

| App type | Example | Privacy policy | Enforces HTTPS |
| -------- | ------- | -------------- | -------------- |
| Hybrid | [Manchester libraries](https://play.google.com/store/apps/details?id=uk.co.solus.manchesterlibraries) | [Policy](http://www.yourlibraryapp.co.uk/Privacy.htm) | Yes |

Solus supply many UK authorities with a library membership app.  The app is built using [PhoneGap](http://phonegap.com/)/[Apache Cordova](http://cordova.apache.org/).

Solus release an app per authority, though the underlying app and web files largely largely the same.  The primary changes are in some custom design to match the local library service.  An individual **authority ID** is also passed to a set of web services during various data requests (login, get loans, renew, etc).

Solus choose to access each individual authority from behind the security of their own infrastructure, exposing publicly just their own web service.  This allows them to access a variety of different types of library technology suppliers, without putting all that logic into the app or giving away details of those web connections that go on behind the scenes.

As of 2015, the URL used to access those web services is an HTTPS one, encrypting credentials when using the app.  As Solus pass on requests to each relevant library authority it is not guaranteed that they will be encrypted for the whole journey, but that is largely out of their hands.

The privacy policy gives details of policy on adware, spyware, what data is stored, colection of usage stats, and user options.

Axiell
------

| App type | Example | Privacy policy | Enforces HTTPS |
| -------- | ------- | -------------- | -------------- |
| Native | [Axiell My Library](https://play.google.com/store/apps/details?id=dk.bridgeit.axiell.mylibrary&hl=en) | None provided | No |

Axiell are a library technology supplier providing solutions such as *OpenGalaxy*.  The Axiell My Library app takes a different approach to the Solus one.  Rather than provide an app for each authority, the app lists all participating authorities and the user chooses their own.

The Axiell app also differs from the Solus one in that it does not use a single web service, it accesses a web service direct to each authority.  This brings more of the logic into the app itself, but also makes the process more efficient - an intermediate web service creates an extra step to retrieving data.

Because the Axiell app uses individual web services for each library, the answer to whether data is transmitted securely is *it depends*.  Some authorities have web services which are HTTPS, others use just HTTP (much like their web catalogues).  This makes it a slight gamble for the user as to the level of security they are provided with.

Sirsi Dynix
-----------

| App type | Example | Privacy policy | Enforces HTTPS |
| -------- | ------- | -------------- | -------------- |
| hybrid | [BookMyne](https://play.google.com/store/apps/details?id=sirsidynix.bookmyne) (e.g. Sutton Council) | | Yes |

Sirsi Dynix also have a single app to access services across multiple libraries.  Any library service that have [Sirsi Dynix web services](http://www.sirsidynix.com/products/api-web-services) installed can make use of the BookMyne mobile application.

In this case the app is a hybrid app, displaying web pages stored on the device.  There is a slight difference though - the web pages themselves are downloaded and saved from the Internet.  An approach that means the code can effectively be changed without upgrading the app itself.

BookMyne also use a set of web services under an HTTPS address, so data is transmitted securely.

Civica - Sorcer mobile
----------------------

| App type | Example | Privacy policy | Enforces HTTPS |
| -------- | ------- | -------------- | -------------- |
| Native | [Sorcer](https://play.google.com/store/apps/details?id=com.civica.sorcer) (e.g. Lincolnshire libraries) | None provided | No |

Civica provide Sorcer mobile for users to access their library account.  Another single library app, this integrates with authorities who are using [Spydus](https://www.civica.co.uk/library-and-learning/spydus_opac).

The app is written in native code (e.g. Java for the Android version).  Like the Axiell app, it directly accesses different URLs for each registered library.  Again, these appear to be a mix of HTTP and HTTPS, so the security of the app is dependent on which service the user is member of.

Civica - Spydus mobile
----------------------

| App type | Example | Privacy policy | Enforces HTTPS |
| -------- | ------- | -------------- | -------------- |
| Native | [Spydus mobile](https://play.google.com/store/apps/details?id=com.civica.sorcer) (e.g. ) | None provided |  |

Civica seem to have another app providing integration with Spydus.

Infor
-----

| App type | Example | Privacy policy | Enforces HTTPS |
| -------- | ------- | -------------- | -------------- |
| Hybrid | [Iguana Library](https://play.google.com/store/apps/details?id=com.infor.libraries.iguana.mobile&hl=en) (e.g. Warwickshire libraries) | [Policy](http://infor.colo.ba.be/iguanamobile/www.index.cls#description) | No |

Infor systems have an Android/iOS/Windows app for the Iguana web portal.  Again, this allows integration with any authorities that use Iguana web catalogue.

The privacy policy says nothing about how data is stored or transferred, it lists functions of the app.

Future developments
-------------------

The current state of mobile app security standards (despite apps being in development for quite a few years) can be said to be immature compared with web security.  Issues for users such as unencrypted web services, or allowing encryption for only some users, highlight security concerns cannnot be left to the good will of suppliers.

As browser competition has pushed up web security standards, operating system providers (Android/iOS/Windows) need to provide similar standards for their users - alerting users when any web services are being accessed that are not encrypted, and making unencrypted traffic an explicit permission that the app needs to request.