This page is meant to be concise and readable from the top down, without skimming.

If you don't have any experience with python, please follow one of the many tutorials available on the internet. To follow this guide, you will need to have a basic understanding of the syntax of python, know how to run a python file, and a have working python installation.

If you have not yet [installed](Installation.md) nxt-python, please do so before trying to use it. Once it's installed, make sure your NXT is turned on and that you can connect to the brick with either bluetooth or USB. If you've got NXT-G set up, you should arrange things the way you do when using it.

First, look at the .py files in the examples directory of the package or svn trunk you downloaded. Try running one or more of them (In Windows, open with IDLE and then press F5). If you don't get any errors and the brick does something, your installation is working. If you do run into problems which you can't fix on your own, try asking for help at the [mailing list](http://groups.google.com/group/nxt-python).

The first thing you should after running the example is change the line with `b = nxt.locator.find_one_brick()` (or something similar) to pass name = 'brickname' to the function, e.g. `b = nxt.locator.find_one_brick(name = 'MyRobot')`. This will stop nxt-python from accidentally connecting to stuff like mice, phones, and random laptops.

Once you have run an example, try changing it to get different results. For example, try making the spin.py file run the motors for longer or use different ports. Or, combine elements of the mary.py and sensors.py scripts so that the robot plays different tones based on the distance an object is from the ultrasonic sensor. The more experimentation you do, the better.

Once you understand how the examples are controlling the brick, you can start making your own scripts from scratch. The code in the examples is mostly boilerplate stuff and therefore in public domain, so feel free to copy it into your projects, regardless of their licensing.

If you have any trouble with this document, please refer to [Issue 4](https://code.google.com/p/nxt-python/issues/detail?id=4) in the bug tracker or ask around in the mailing list.