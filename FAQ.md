# How can I control motors while reading sensor inputs? #
Using v2.1.x+ (or the latest SVN), use the thread module to run multiple threads, one for each thing which you want to do simultaneously. This is complex, but a good example of one way of doing this can be found in the examples directory and is called cnc.py. If you must use an older version of NXT-Python, a patch to allow multiple threads can be found at [this thread](http://groups.google.com/group/nxt-python/browse_thread/thread/db9fa1da34c4d103) on the mailing list.


# How do I compile nxt-python scripts to rxe code and run them without a computer? #
You can't. Try [pynxc](http://code.google.com/p/pynxc/).


# How can I make find\_one\_brick() on bluetooth faster? #
You will need the MAC address of your brick. If you don't have this, connect to it and run
```
print b.get_device_info()[1]
```
Then, replace your get\_one\_brick() call with this (don't forget to import nxt.bluesock):
```
b = nxt.bluesock.BlueSock('00:16:53:0E:7E:DE').connect() #replace the string with the one you got earlier
```
You can also find the MAC address of your brick by using the on-screen menu. From the main screen, go to _Settings_, _NXT Version_. The number beside _ID_ is the MAC (just add the colons). It should look similar to this:
> ![http://www.mindstorms.rwth-aachen.de/trac/export/head/branches/atorf/nxt_menu_mac.png](http://www.mindstorms.rwth-aachen.de/trac/export/head/branches/atorf/nxt_menu_mac.png)

Experiments have shown this to reduce the bluetooth brick connection time from about 20 to less than 5 seconds. This is because the bluetooth device search is skipped. This method should work with lightblue as well as pybluez but as only been tested on the latter. Your results may vary.