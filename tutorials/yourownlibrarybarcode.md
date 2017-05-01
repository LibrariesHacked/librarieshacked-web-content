---
Title: making your own library account barcode
Description: a new library app advertises an on screen barcode - but you can do your own
Date: 2015/01/06
Type: Tutorial
Author: dave
GitHub: 
TutorialComplexity: low
TutorialSkillsRequired: JavaScript
TutorialNeeded: a mobile device
TutorialTime: 30 mins
TutorialType: WebDevelopment
Template: tutorialpageyourownbarcode
---

A recent news story announced that Calgary Public Library were about to release a new smartphone app to replace membership cards.  By displaying a barcode on a phone.

- [Calgary Public Library launches Smartphone App to replace library cards](http://mobilesyrup.com/2014/12/19/calgary-public-library-launches-smartphone-app-to-replace-library-cards/)

It's already done by the Solus apps in UK libraries, and can be easier for a user to do themselves than to download a whole app for.  (Though an app may offer a lot more as well).

The immediate option would be to take a photo of your card, or scan it.  But if you don't want a dodgy photo, and want to create your own barcode, it's not difficult to find websites to create a barcode image from text. The main complexity is there are a variety of different standards and types of barcodes.  It's likely enough that your library will scan **code 39** type barcodes, but if that doesn't turn out right it may be a different barcode format (such as **code 128**).

The [jQuery barcode generator](http://www.jqueryscript.net/other/Simple-jQuery-Based-Barcode-Generator-Barcode.html) provides JavaScript code to generate a barcode in a variety of standards, allowing websites to provide barcode generators on their site for users.

When the barcode is generated there's also a chance to get creative with the image.  An app means you have to unlock your phone, launch the app, and get to the barcode.  Creating a lock-screen wallpaper out of the library barcode would mean the phone/tablet doesn't even need to be unlocked to be used as a replacement library card.

**Note: Do check out the security of your individual library with regards to whether they enforce a membership number (barcode) AND password.  A lock screen wallpaper may not be the best option if the barcode will gain access to your account.**