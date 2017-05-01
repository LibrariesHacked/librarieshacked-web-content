---
Title: making sure your hack sites can't be hacked
Description: WordPress is a popular platform for hack sites - but it leaves security to the administrator.
Date: 2014/07/07
Type: Tutorial
Author: dave
GitHub: 
TutorialComplexity: low
TutorialSkillsRequired: WordPress,security
TutorialNeeded: a WordPress site
TutorialTime: 30 mins
TutorialType: WebDevelopment
Template: tutorialpage
---

It's difficult to know how many websites use WordPress, but various reports suggest it's around 20% of all sites:

- [19% of the web runs on WordPress](http://venturebeat.com/2013/07/27/19-percent-of-the-web-runs-on-wordpress/)

On the events page of this site it's considerably more (though a small sample): currently 6 out of 9 of the links are to WordPress sites.  That may be indicative of the nature of those sites - often relatively small sites giving information about hack events and projects.  WordPress is known for being used as a personal blog platform, but it is also popular for hacks as it is easy and quick to set up, with a huge number of free themes and plug-ins.  And though the standard WordPress templates can make them instantly recognisable, a WordPress site also doesn't have to look like a WordPress site - many web development and design agencies are now WordPress development and design agencies, creating themes and plug-ins for clients to use.

Using WordPress is hardly a lazy option, but it is one that needs more work than is often considered.  One of the main reasons is security, WordPress itself doesn't have severe security vulnerabilities (various plug-ins do), but it puts managing security in the hands of users.  When those users are largely people who want a quick and painless way of getting an easily manageable site up and running, with minimum hassle, that can be a problem.

Here are some quick tips it's worth thinking about if you're a WordPress administrator and hacking together a quick site.

Tip 1. Keep usernames secret
-----------------------------

It's often not considered important to keep a username secret because people often use email address as username.  And that's often public, why should sharing your WordPress login be any more serious than sharing your email address?  The problem is that most major email providers have security measures in place to lock down your account if too many attempts are made to access it.  In that way, it doesn't matter if other people know the username because they can only try so many times to get access.

A basic WordPress installation won't make such decisions for an administrator on how many times users are able to try to login.  That's left to the administrator and, without any additional security, if a username is known then the site will be vulnerable to 'brute force' attacks (trying different passwords over and over again).  There are automated tools able to target a site and attempt to login in this way, and if the site doesn't stop that happening there's a significant chance the attack could succeed.

If you have a WordPress site it's worth navigating to the following page to see what information on usernames can be found:

- http://[yoursite]/?author=1

In many cases that redirects to a page showing the username of the first user (often 'admin').

### don't display usernames in WordPress

One way of giving away usernames to attackers is by listing them on the site pages.  There is a setting in WordPress to use nicknames rather than usernames when displaying users.

### don't allow WordPress login to reveal any secrets

If you go to a WordPress login page, type in your username with an incorrect password what message does it give?  It will probably be something like:

**ERROR: The password you entered for the username admin is incorrect**

Which is effectively WordPress responding: **yes, that's half-right, but the password is wrong**.  Even if the WordPress pages are not listing usernames in them, a simple login attempt can confirm whether certain usernames are being used.

You could search through the website code and change that text, or there are security plugins that will display a more standard message.  

### don't use the default user names

None of these things show any definite vulnerabilities in WordPress, they just assume that users will not show usernames in pages, and that they will not use usernames that could be guessed.  The most important thing is to not use a username like 'admin' that can easily be guessed and confirmed to be correct.

### force strong passwords

There is an argument that usernames should not be secret, as ultimately a strong (complex) password will keep out any attack.  It's true that this is most important, but if keeping a username secret stops an attack in the first place it still seems worthwhile.  An administrator doesn't always have control over user passwords, so this is one option they can manage.

If you do want to control user passwords, there are plugins available that will force users to at least enter a strong password.  

Tip 2.  Perform upgrades
------------------------

There was a time when WordPress was actually full of vulnerabilities and had a drive to deal with their security issues.  These have always come in the form of updates, but these need to be installed by someone with an administrator account.  It's important to make sure the system is kept up to date so that any vulnerabilities that are discovered can be fixed on your site.

Tip 3.  Backup
--------------

Backup hardly needs to be said, but it's worth considering how disastrous it would be if the worst happened.  In the case of a commercial organisation the outcome can be severe loss of income, loss of customer data, and heavy fines.  For the majority of library blogs, events, and information sites the worst is probably that the site is deleted, and that relevant usernames and passwords are compromised.  That shouldn't matter too much.  As long as you have a regular backup, you can simply re-install WordPress and restore a copy of the database. usernames and passwords should be ones that aren't used elsewhere.

Most web hosts offer some kind of automatic backup system that you can use as part of their admin console, but unless you've kept a backup most won't restore your database if it's been deleted (though they most likely will have a backup somewhere - it's often just not part of their service).

You can also use WordPress backup plug-ins which will be able to take a regular backup of your database and email it to you.  WordPress also has an option to export content, which isn't quite the same as a complete backup but does mean that posts and pages could be restored within a new install.

Tip 4.  Install a security plug-in
----------------------------------

This isn't an advert for any particular security plug-in, but there are many good ones available for free within the plug-ins directory.  Simply search for them and choose one that is popular, has good ratings (and check out it doesn't have it's own vulnerabilities), and does what you want it to do.

A list of common features are:

- options to turn on captcha features on the login page, ensuring automated tools can't be used.
- tools to change the page name of the login page so that it can't be found (except by you).
- limiting the number of login attempts.
- automatic blocking of known robots and IPs.
- automated backup.
- enforce strong passwords.

Tip 5.  No-one's stupid or an expert
------------------------------------

Like most blogs and guides this comes from some hard experience from not doing any of these things.  It's not an expert guide - when it comes to online security no one knows all the answers.  Similarly, to not have done any of these things, or to use admin as username, is not stupid.  There are just good reasons for not doing so that it's worth communicating.

Tip 6.  Everyone is a target
----------------------------

One unfortunate thing about the internet is that everyone is a potential target of hacking because it is so often automated.  A community group may think *no one's going to target us*, and find out the hard way when an online bot brute forces into their account and deletes the site, or just adds links to dodgy substances that destroy any work done on rankings in search engines.