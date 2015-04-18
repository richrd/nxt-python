# NXT\_Python #
The most recent releases are at the bottom.

## Version 0.x ##

### Version 0.1 ###
Initial release

### Version 0.2 ###
USB support added

### Version 0.3 ###
Ultrasonic sensor support added

### Version 0.4 ###
Improved sensor support + example programs

### Version 0.5 ###
Fixed timing problem with USB and Ultrasonic sensor

### Version 0.6 ###
Added nxt.compass module for reading Mindsensors compass.
Added optional "host" and "name" arguments to find\_bricks and find\_one\_brick methods. This allows multiple bricks to be controlled at the same time. (Thanks to Simon Levy!)
Added null-termination to messages for write\_message.

### Version 0.7 ###
Added a get\_sample() method to CompassSensor class.
Simplified return values for message\_read() method.
Added message\_test.py to examples directory.

# NXT-Python #

### Version 0.8 ###
Added server module, for a socket-based driver-like command-driven interface.
Some better help messages.
Added a logo! Yay!
More messages when brick.debug is set to 1.

#### Version 0.8.1 ####
Added prelimenary support for accelerometer sensors.
Important bugfix and some more commands for server.py.

### Version 0.9 ###
Now licensed under the GPLv3 or later.
Slightly improved accelerometer class.
A few improvements to nxt.server module.

## Version 1.x ##
Note: At this point I was still getting the release numbering scheme sorted out. The 1.0 and 1.1 series were originally released as v1.0 and v1.1, and remain that way in setup.py and the package download section (though they are no longer listed, they can still be found at their respective URLs). However, they were retroactively changed to v1.0.0 and v1.1.0 in the SVN repository to promote uniformity in the v1.x series. From v1.2.0 and onward, the initial release in a minor series includes the trailing '.0' in every field.

### Version 1.0.x ###

#### Version 1.0 (svn/tags/1.0.0) ####
Experimental support for motor braking.
Some improvements to the server module, like multiple motor control.

#### Version 1.0.1 ####
Motor braking bugfix by Steve Castellotti.


### Version 1.1.x ###

#### Version 1.1 (svn/tags/1.1.0) ####
New motor functions, namely run() and stop().
Motor braking is finally usable!
Slightly changed behavior when there are errors searching for a brick.
New server functionality for light sensor illumination.

#### Version 1.1.1 ####
Fixed missing examples directory problem.

#### Version 1.1.2 ####
Fixed the timing problem when using a low-speed I2C digital sensor (e.g. Ultrasonic) with USB.

### Version 1.2.x ###

#### Version 1.2.0 _(current stable)_ ####
Added native Mac support.
Added support for the HiTechnic Compass sensor (thanks to Samuel Leeman-Munk).
Improved the fix for the USB digital sensor timing bug (see v1.1.2).

## Version 2.x ##

### Version 2.0-beta-x ###

#### Version 2.0-beta-0 ####
Huge, huge numbers of changes made, sensor module completely rewritten by rhn, motor braking overhauled, synced motor support added, server module removed (see comments [here](http://code.google.com/p/nxt-python/wiki/ServerUsage), tons of functions have different names and do different stuff. Most of the API changes have been made at this point.

#### Version 2.0-beta-1 ####
Updated examples to work with new code, removed included html changelog (replaced with this page), perfected motor braking, project-wide indentation change from tabs to four spaces, reformatted get\_sensor\_info() to be more readable across copy+paste cycles (there were problems with binary chars in the info strings), changed all sensors to default to no type checking, rephrased type checking failure dialogue, added some of Samuel Leeman-Munk's MindSensors Compass methods, added more sensors to the type checking database, fixed the HiTechnic Gyro sensor, fixed some copyright dates, and fixed the missing LICENSE and README files issue.

### Version 2.0.x ###

#### Version 2.0.0 ####
Lots of changes to the sensor module, finished digital sensor type checking and enabled it by default, polished everything a bit more, the README was basically entirely rewritten, tried to improve digital sensor performance, fixed the scripts (broken for a while now), lots of new sensors, quite a few MindSensors examples, wrote a digital sensor unit testing system called [nxtduemu](http://code.google.com/p/nxtduemu/).

#### Version 2.0.1 ####
Fixed support for Mac and other systems using LightBlue. Mac support is now confirmed working!

#### Version 2.0.2 ####
Fixed the license declaration in setup.py for the fedora downstream guys. Fixed the HiTechnic Compass and EOPD sensors. Fixed the mindsensors LineLeader example.

#### Version 2.0.3 ####
Fixed the random DirProtErrors when polling digital sensors. Fixed the error which occurred when one sent commands to a sensor in quick succession.

### Version 2.1.x ###

#### Version 2.1.0 ####
Added multithreading capability, support for 2 new HiTechnic sensors, hostname parameter in in scripts, silent mode in nxt.locator module, alpharex example, and cnc example. Tweaked the digital sensor code for 2x faster sampling on USB (Bluetooth is still slow).

### Version 2.2.x ###

#### Version 2.2.0 ####
Largest change since 1.x-2.x. Revamped nxt.locator module find\_bricks and find\_one\_brick logic to allow easy addition of more communication backends and external configuration file support. Added support for [Linus Atorf's RWTH - Mindstorms NXT Toolbox Motor Control](http://www.mindstorms.rwth-aachen.de/trac/wiki/MotorControl) program through the nxt.motcont module. Added support for the LEGO Fantom driver via TC Wan's pyfantom module and nxt.fantomsock. Added a new byte-level server interface which is free from all the problems which plagued the 1.x server module. Added docstrings to brick methods. Added manpages and --help messages for the nxt scripts. Chagned nxt\_test to only look for one brick. Optimized opcodes which are not reply-required to not wait for a reply.

#### Version 2.2.1 ####
Fixes a major issue with USB on windows and two minor issues in the fantom backend and the push script.

#### Version 2.2.2 _(current stable)_ ####
Raises a timeout value in the digital sensor control code and fixes a function name in the fantomsock module.