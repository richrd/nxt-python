nxt-python is a python driver/interface for the Lego Mindstorms NXT robot. The 1.x releases aim to improve on NXT\_Python's interface and should be compatible with scripts which use it while the 2.x releases improve on the API in backwards-incompatible ways and will not work with NXT\_Python scripts.

# Getting started #
To start controlling your lego nxt robot with python, just follow the setup instructions on the [Installation](Installation.md) page.

### News ###

May 25, 2012: Bugfix version 2.2.2 released. Just a few minor comms things affect a very small number of users.

Nov 20, 2011: Release of bugfix version 2.2.1. Fixes a major issue with USB on windows and two minor issues in the fantom backend and the push script.

Jul 7, 2011: Release of major version 2.2.0. Lots of new features including MotorControl support, Fantom driver support, brick location configuration file support, and much more. Read about it on the [[Versions](Versions.md)] wiki page and get it at the Downloads tab, Downloads bar, or Python Package Index.

[Older News](http://code.google.com/p/nxt-python/#News_Archives)

## About nxt-python ##

nxt-python works by sending the direct commands detailed in LEGO's "Bluetooth Development Kit" to the nxt brick, and on occasion providing a greater level of abstraction. You can't compile and download code to run on the brick as in NXC. In the sensor module, support has been added for many third-party sensors using protocol information from the sensors' manufacturers or by reverse engineering demo programs for other languages.

NXT\_Python (which nxt-python is based on) was written by Douglas P Lau back in May 2007. As it is nearly certain that no more updates will be made there, I have copied it here (under the GPL) with a slightly different name where it may enjoy active development and better documentation.

Note: The original NXT\_Python 0.7 zipfile and tarfile are available on the downloads page.

The code here is released under the Gnu GPL v3 or later and the documentation under the Creative Commons v3 or later. Example code snippets can probably be stolen unscrupulously.

Doing something cool with the library? We'd love to hear about it! Send an email to [me](http://code.google.com/u/@UxFTQlVWBxNAXQl9/) or the mailing list and tell the story.

# About v2 #
On September 30, 2010, packages for version 2.0.0 of NXT-Python were added to the downloads page. This version (often referred to as "v2") is backwards-incompatible with previously written programs, which means that pre-existing nxt\_python or nxt-python scripts will NOT work with it. However, it has some cool features along with a better interface for you to use. When starting new projects, you should use one of the new packages marked "v2.0.0". Unfortunately, upgrading programs to use the new interface is difficult and not recommended. Note that the v1.2.x series is still being bugfixed and supported, so things should be fine.

Here is a list of the new features:
  * New sensor interface, including better internal workings of the sensor module and great digital sensor framework. This means no more compass-sensor-in-a-different-module stuff.
  * Automatic detection of sensor types and creation of the correct sensor object for the sensor. After connecting to a brick, use its `get_sensor()` method to identify and use a digital sensor connect to a specified port.
  * New motor interface! With improved (but still rather inaccurate) braking support and an experimental synchronized motor class.
  * No more server module! This thing was a pain to maintain/use, and wouldn't be feasible in a real environment.
  * Support for 24 different sensors, including 8 from [HiTechnic](http://www.hitechnic.com/) and 11 from [MindSensors](http://www.mindsensors.com/). See nxt/sensors/init.py for a complete list in python-syntax format.
  * You no longer have to use connect() on a new brick object before using it because this is now done automatically.
  * A unit test system for emulating digital sensors' I2C interface behavior for testing without the actual hardware. This has been split off into [nxtduemu](http://nxtduemu.googlecode.com/) and is only included in SVN checkouts, not packages.

As always, there is room for more features which don't involve changing existing API methods. You can use the bug checker to make suggestions.

### News Archives ###
Feb 13, 2011: Release of feature version 2.1.0. There are quite a few things fixed, two new sensors, silent mode for the locator module, a 2x polling frequency increase for digital sensors on USB, and of course multithreading support. It can be found at the sidebar, downloads page, or the new [PyPI page](http://pypi.python.org/pypi/nxt-python/2.1.0). In addition, I seem to have neglected to document the release of v2.0.3 here. It includes some of the digital sensor fixes from the trunk, was released on the 6th, and is now superseded by this version.

Feb 3, 2011: Release of bugfix version 2.0.2. It fixes some hitechnic sensors and the license declaration in setup.py. Get it at the downloads page.

Jan 26, 2011: We're not dead yet! zonedabone is working on a branch where there are some key redesigns to the inner workings of the communiction system, jarradgenson is working on getting the HiTechnic sensors fixed and comfirmed working, fmc3 is hunting down a bug with digital sensor queries, and we have just decided that there will be no Windows executable installers built for upcoming releases (the rationale may be found on the mailing list). This is accompanied by the related news that we now have an entry in the PyPI and support easy-install. A 2.0.2 bugfix release reflecting jarradgenson and fmc3's bug hunting is upcoming, probably within the next week or two.

Oct 31, 2010: Halloween Update (since I had nothing else to do today!): The SVN trunk now supports optional multithreading! Set a brick's threadsafe property to true to enable it (this variable may be changed whenever the connection to the brick is idling) [EDIT: With newer SVN versions, threadsafe mode is always on]. This works for both USB and BT.

Oct 26, 2010: v2.0.1 is released to fix the problem with Mac and other systems using LightBlue. If you are not using LightBlue, there is no need to update.

Oct 25, 2010: Da3v has reported that nxt-python is currently broken on Mac. I have found the problem and created a patch to resolve the issue and will be releasing bugfix version 2.0.1 after the fix has been reported working.

Sep 30, 2010: After 8 commits worth of fail, v2 is here. Everyone party! More info in the updated v2 section. Check it out over at the downloads page or grab the SVN.

Sep 27, 2010: I have created a unit testing system for the digital sensor code called nxtduemu. It may be found at the svn repo over at the [project page](http://nxtduemu.googlecode.com/) or in the arduino directory of an nxt-python working copy (but not a release). See [arduino/README](http://code.google.com/p/nxtduemu/source/browse/trunk/README) or the aforementioned link for more info. Any nxt-python developer will be granted svn write access on request, and improvements are welcome from anyone.

Sep 26, 2010: The the v2 sensor code now also supports the HiTechnic Sensor Prototype Board. As always, testers welcome!

Sep 1, 2010: The v2 sensor module now supports almost every HiTechnic and MindSensors device, the HiTechnic ones partially thanks to me and the MindSensors ones mostly to due Ryan Kneip from mindsensors.com. Many thanks to him and the rest of the guys at mindsensors.com!

Aug 20, 2010: I added a script which helps to report sensor identification information to me for use in the digital sensor type checking database. If everyone could check out the latest svn and run the nxt\_sensor\_report script and follow the directions, it would really help out. It takes about 5-10 minutes and is an easy way to help with development even if you can't do coding.

Aug 18, 2010: With the release of v2.0.0 hopefully only a few weeks away, v1.x is about to go into permanent feature freeze mode so that the trunk can house the developing v2 code. The freeze is currently scheduled for the 23rd, but can be pushed back a bit if anyone has new features in the works. See the full plan on the mailing list (link in sidebar). `[`EDIT: The feature freeze was completed on schedule and the svn/trunk now holds the v2 code.`]`

Aug 16, 2010: v2.0-beta-1 is released. Get it over at the downloads page. It's really only for testing, so if you want to help out please see the "note regarding v2" below.

Aug 16, 2010: v1.2.0 is released. Yay! Amongst other things, there is native Mac support, support for the HiTechnic Compass sensor, and a better fix for the USB ultrasonic sensor timing bug.

Aug 14, 2010: The trunk now has support for the HiTechnic Compass sensor! Thanks to Samuel Leeman-Munk. Look for this feature in v1.2.

June 26, 2010: There is a beta of the v2 branch available over at the downloads page. I encourage you to read the note about v2 below and then do some testing with one of the beta packages. Remember, if you see something you want changed, it's never too late to say so (except in the case of API changes, where sooner is better than later)!

June 24, 2010: The svn now has support for the HiTechnic Gyro sensor as well as the NXT 2.0 Color sensor. Thanks, melducky!

June 7, 2010: Version 1.1.2 is available at the downloads section or handy-dandy sidebar. This bugfix version corrects the operation of the Ultrasonic Sensor when connected to the brick with a USB cable.

February 5, 2010: Added [installation instructions for Mac OS X](http://code.google.com/p/nxt-python/wiki/Installation#Mac) integrating the new internal LightBlue Glue. Thanks to Simon Levy for letting us use it!

January 27, 2010: Bugfix version 1.1.1 is released to remedy the fact that there is no examples directory in the packaged version. Sorry to all who didn't get any examples with their download...

November 20, 2009: Version 1.1 is released! Grab it over at the downloads page, or just click on it under the featured downloads tab to the left.

November 2009: rhn is redoing the sensor module, look for a new, backwards-incompatible interface in v2.0!

July 24, 2009: Release of version 1.0.1 (changelog in trunk/docs of svn).

July 24, 2009: Motor braking bugfix by Steve Castellotti.

May 20, 2009: Release of 1.0 (changelog in trunk/docs of svn). Work on motor braking.

Apr 7, 2009: Release of 0.9 (changelog in trunk/docs of svn).

Apr 5, 2009: Added optional password protection to server module. See [ServerUsage](ServerUsage.md).

Mar 27, 2009: Release of 0.8.1 (Important bugfix of 0.8).

Mar 25, 2009: Paulo Vieira contributes class for accelerometer. Many thanks!

Mar 22, 2009: Official release of 0.8. See the downloads tab (above). [Detailed changelog](http://code.google.com/p/nxt-python/source/list?start=32)

Mar 20, 2009: Finished the server module. See [r28](https://code.google.com/p/nxt-python/source/detail?r=28).

Mar 19, 2009: Finally, a (partially) functional server module. TODO: Implement input data for motors and playing tones, and document it. Obviously, the former comes first. See [r23](https://code.google.com/p/nxt-python/source/detail?r=23). Note that there will be NO backwards compatibility at this point if there is a change to the socket interface, so don't publish any applications relying on it yet.

Mar 17, 2009: More helpful help messages, see [r20](https://code.google.com/p/nxt-python/source/detail?r=20)
Mar 14, 2009: Compiled an executable installer for PyUSB for use with Python 2.6. See the downloads tab (above) to get it. Also added more debug messages and a number of small changes in [r17](https://code.google.com/p/nxt-python/source/detail?r=17).

Feb 24, 2009: Renamed the readme and license files. See [r14](https://code.google.com/p/nxt-python/source/detail?r=14).

Feb 18, 2009: Added simplified motor syntax. See [r13](https://code.google.com/p/nxt-python/source/detail?r=13).

Feb 6, 2009: Got the preliminary documentation up...nowhere close to done, but it's there. See [Documentation](Documentation.md).