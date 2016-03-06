/*
Title: automation on mobile operating systems
Description: using sl4a to make use of mobile device features and automate tasks.
Date: 2014/07/25
Type: Tutorial
TutorialType: MobileDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: python,scripting
TutorialNeeded: an android device, a python text editor of your choice
TutorialTime: 1 hour
Template: tutorialpage
*/

The shift to using mobile operating systems like Android and iOS has led to an increased use of phones and tablets effectively as mini-PCs, with the ability to perform the same productivity tasks PCs are used for.  The keyboards are more fiddly, the screens smaller (although often with greater resolutions than many monitors), but it's easy enough to cast the display onto a TV and add a bluetooth mouse/keyboard.  For many people mobile tech is able to replace their old PC, with add on devices only used when necessary.

One limitation of mobile operating systems is that they don't offer the same level of automation for *power-users* that a desktop OS will.  On a desktop there are countless ways of programming and scripting tasks, using any language of choice (for Mac, Windows, or Linux).

Both Android and iOS allow users to develop apps and deploy these to their own devices, but this requires creating whole apps rather than just the simple scripting of tasks for automation.  There are also plenty of apps that allow tasks to be scheduled, but these don't offer the flexibility of real scripting.  Rather than choosing from a list of pre-set tasks, users need to be able to create scripts that do exactly what they want - including automating many processes that will have no relevance to other people.

Apple that make user scripting difficult - no app would even be allowed on the app store that could download and run code;  apps are also *sandboxed*, without integration with other files or apps on the device - so sharing data between apps becomes difficult.

Despite this, there are still options for automation in iOS.  The excellent blog post, [Automating iOS: How Pythonista Changed My Workflow]( http://www.macstories.net/stories/automating-ios-how-pythonista-changed-my-workflow), demonstrates using Pythonista, a beautiful looking iOS app for automating tasks, and provides a discussion of workarounds for the security restrictions.

For Android, one option is *SL4A* - scripting language for android.  This allows scripting using a number of different languages, as well as giving direct access to the device functionality - such as location and camera.  The example below takes a quick look at using SL4A to use the device location sensor and integrate with an API - the culture grid web service.

## step 1 - install and set-up the apps

- you'll need to make sure your android device allows you to install apps that aren't from the Google Play store.  This is a phone setting usually found in the security section.
- install the SL4A app by navigating to [this link to sl4a](http://code.google.com/p/android-scripting/) on your device.
- also install the Python for Android app.  This can be done from within sl4a.  launch sl4a, and in the menu navigate to *view* > *interpreters*.  Then launch the menu again and select *add*, and then *python 2.6.2*.  This will launch the download for python for android.
- once installed, launch python for android and click install.  You're ready to script with python within SL4A.

## step 2 - create a script
This example will find the current location and then call to the culture grid web service to retrieve the nearest library and display it as a notification on the device.  Not necessarily the best use of automation for productivity - but one that demonstrates using the android API and culture grid.

- launch SL4A and from the menu select *add* > *python*.  This launches a text editor where you can write your python script.  Rather than go through line by line, the code below is commented throughout.

<pre class="prettyprint linenums">
<code># three python imports are necessary for this.  android, urllib (for calling the web service), and ElementTree (for dealing with the xml data returned)
import android, urllib, time, xml.etree.ElementTree as ET

droid = android.Android()

# trigger location provider in android
droid.startLocating()

# give it 5 seconds to get back a location 
time.sleep(5)

# time is up - read the location from the device
location = droid.readLocation().result

# if nothing back (location = {}) then grab the last known location on the device
if location == {}:
	location = droid.getLastKnownLocation().result

# the location provider will return a set of locations from different methods (gps, network)
# this code loops through each one, analyses the time, and finds which one is the latest.
providerToUse = ''
latestLocation = 0

for provider in ['passive', 'network', 'gps']:
	if provider in location:
		if not location[provider] == None:
			if 'time' in location[provider]:
				if location[provider]['time'] >= latestLocation:
					providerToUse = provider
					latestLocation = location[provider]['time']

# if it still hasn't found a location at all, notify the user and exit.
if latestLocation == 0:
	notify("location not available", "could not find your location, try turning on gps or wifi")
	exit()
					
# now take the latitude and longitude from the latest location provider					
latitude = location[providerToUse]['latitude']
longitude = location[providerToUse]['longitude']

# now call to the web service to get hold of the data
# in the web service we'll set the following options:

numOfResults = 1
fieldsToReturn = "dc.titleString,institution_address"

# from those options construct a url - the lat and lng need to be cast to a string (str)
url = "http://www.culturegrid.org.uk/index/select/?q={!spatial%20lat=" + str(latitude) + "%20long=" + str(longitude) + "%20radius=20%20unit=miles}institution_sector:Libraries&rows=1&fl=" + fieldsToReturn

# now get back the data.  this uses the urllib python library to open a url and read the data from it.
data = urllib.urlopen(url).read()

# the data will be returned as xml
root = ET.fromstring(data)
libraryName = ''
libraryAddress = ''
libraryLat = ''
libraryLng = ''

# query the xml - this is done by finding all elements of the name 'str' and then checking the attribute name
# e.g. &lt;str name='dc.titleString'&gt;library name&lt;/str&gt;
for e in root.findall('.//str'):
    if e.attrib.get('name') == 'dc.titleString':
        libraryName = e.text
    if e.attrib.get('name') == 'institution_address':
        libraryAddress = e.text

# we have all the data - throw it into a toast notification to tell the user
droid.notify(libraryName, libraryAddress)
</code>
</pre>

## step 3 - publish the script as a shortcut
Once that is done it can be tested within SL4A.  To save a script when in the editor launch the menu and select *save and run*.  The script can also be launched by tapping it when on the home page of the app.

For convenience, the script can be added as a widget on the device.  Add a widget and select SL4A - this will give you an option to select the script you wish to run without having to launch the sl4a app.

## further information
There is an excellent list of tutorials for SL4A on their wiki.  This includes a couple of library related ones - looking up books on a library catalogue site, and looking up books on LibraryThing.  Both implement the barcode scanner on the devices.

[list of tutorials](https://code.google.com/p/android-scripting/wiki/Tutorials)

For scheduling of scripts, there is also a script launcher on Google Play

[script launcher app](https://play.google.com/store/apps/details?id=org.androidideas.scriptlauncher&hl=en_GB)