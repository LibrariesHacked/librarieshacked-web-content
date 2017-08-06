---
Title: how secure is your library app?
Description: do you know if your login and personal details are encrypted while using your library app?
Type: Article
Tags: https mobile security,library mobile apps,android and iOS
Template: blogpage
Author: dave
Date: 2014/11/20
---

*[HTTPS]: Hyper Text Transfer Protocol Secure

#### HTTPS in the browser

Many people are familiar with HTTPS: encrypting communication between your PC and a website.  Various rules are associated with this standard.  Websites need to present a certificate, which needs to be verified by a certificate authority.  A browser would refuse to display a site if the certificate it found was for a different site, or unverified, or expired.  Browsers also provide a reassuring green padlock in the address bar when using HTTPS, giving users some peace of mind when logging into online services.

Users expect encryption (or expect **security**, whether they know about encryption or not) whenever a site provides some kind of login functionality.  Even if the site holds no personal data, the fact that it needs a login means those credentials should be kept secure.  Given the amount of web surveillance, many believe that HTTPS should be used all the time.  [HTTPS Everywhere](https://www.eff.org/https-everywhere) provides browser plugins in order to try to implement this, but the minimum for a web administrator is to ensure that login and personal details are kept secure.

The effect of not using encryption when passing login credentials would be that someone else on the network could be having a nose at network traffic (termed **packet sniffing**).  They would simply be able to take note of the address of the site and the username/password relatively easily.

#### HTTPS in apps

Apps on phones and tablets have few built in security features to make the user aware of the security going on in the background.  The major operating systems (iOS, Android, Windows) give no information about how data is being transferred while using the app, it's left to the trust and imagination of the user.

It is easy to be cautious when using the web.  As soon as you open your browser you are **on the web**, transmitting data back and forth.  When you start using an app you are simply on that app, on your phone/tablet.  You may not even have a permanent internet connection, and any network connectivity is largely going on behind the scenes - if email is being downloaded to your phone do you notice if it is using an encrypted connection?

So much can go on even when an app isn't open.  With the web, even if you know a particular site doesn't have encryption, you might only access it while connected to a secure network.  For individuals that have personal blogs, a security certificate will often cost more than the website.  One imperfect way around this is to only login to your personal blog on a secure network, and to never use a password used elsewhere.  With apps, activity can be triggered whenever a connection becomes available, making it fairly impossible to only use a particular app when on a particular network.

#### Library apps

With the increase in mobile apps for public library users, security is going to become a bigger issue for library authorities.  A local council will already have a fairly substantial set of web security standards - most will have regular **penetration tests**, employing professional security experts to attack their websites and create reports on anything that isn't up to scratch.  Mobile app security is a bit of a black hole in this regard - it is unlikely security is tested to the same standards, if at all.

But if library app usage is going to increase, suppliers need to ensure their security is up to scratch, and not take advantage of users that have little way of knowing what standards are being used.  It will only take a high profile incident to set councils against mobile apps completely - once trust is lost it can be hard to recover.

So how secure is your library app?  The simple answer is probably that there isn't enough information to know, without hooking up our devices to network sniffers.  But if in doubt ask the library before using it.  If they don't know then don't use it.