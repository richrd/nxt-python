# Installation Instructions #

## Linux ##

You will need version 2.6 (or later 2.x) of Python, most modern systems already have this installed. However, as more distributions move to 3.x (which is not compatible with nxt-python), you may need to install version 2.6 in addition (they coexist fine) using your package manager.

Download the latest source package and extract it. Open a terminal and run `cd nxt-python` (or whatever directory you put it in). Then, as root (e.g. sudo) run `python setup.py install`. If the word ‘error’ is not anywhere in the text that is printed, then great. If it is, then follow the error instructions to resolve the issue (or ask about it at the [mailing list](http://groups.google.com/group/nxt-python), and we'll try to help you out).

> For USB communications, download and install [PyUSB](http://sourceforge.net/projects/pyusb/) according to the instructions included in it. If you're on debian/ubuntu, just run `apt-get install python-usb` as root. The following step may not be necessary on some newer systems which give any user control over USB devices by default, but it can't hurt and is therefore recommended. As root, run the command `groupadd lego`, then `usermod -a -G lego <username>` (where `<username>` is the name of the user account that you will be using to program your NXT). Then make a file at /etc/udev/rules.d/70-lego.rules containing exactly (note that after this you will need to log out and then back in and then restart udev or your whole system):
```
SUBSYSTEM=="usb", ATTRS{idVendor}=="0694", GROUP="lego", MODE="0660"
```

> For Bluetooth communications, install [PyBluez](http://code.google.com/p/pybluez/downloads/list), or on debian-based systems, you can just run (as root) `apt-get install python-bluez`. You will also need a compatible Bluetooth dongle, if your computer doesn’t already support Bluetooth.

## Windows ##

You will need Python 2.6 or later (But not 3.x).

Download the latest NXT-Python zipfile from the downloads page and extract it. Go to the new folder and double-click on install.bat. If a black window appears, scrolls some text, and then goes away, the installation succeeded. If it shows an error message and does not go away, there was a problem. If you're not sure if the installation worked, open a python shell and type "import nxt". If it doesn't print any output, things are working fine. Before you get started though, you'll need one of these libraries:

For USB Communication
> Download and install [PyUSB](http://sourceforge.net/projects/pyusb/). USB has a history of being dodgy on Windows, so if the brick can't be found when it should, try bluetooth.

For Bluetooth Communication
> Download and install [PyBluez](http://code.google.com/p/pybluez/downloads/list). You will also need a compatible Bluetooth dongle, if your computer doesn’t already support Bluetooth.

For LEGO Fantom Driver Communication
> Download and install [Pyfantom](http://pyfantom.ni.fr.eu.org/). You will also need to have the fantom driver set up and working.

## Mac ##

To use NXT-Python on Mac, you will need Python 2.6 or higher (but not 3.x).

Make sure you have [PyObjC](http://pyobjc.sourceforge.net/software/) installed; this comes preinstalled with Mac OS X v10.5 (Leopard) and anything newer, but if you have an earlier version of OS X you will need to install it yourself. Then, install [LightBlue](http://lightblue.sourceforge.net/), a python Bluetooth library for Mac. For USB connections, you will need [PyUSB](http://sourceforge.net/projects/pyusb/). You can also use the LEGO Fantom Driver, but you'll need to install [Pyfantom](http://pyfantom.ni.fr.eu.org/) as well. Note that you only need one of these communications backends, but can install as many as you want.

Download the latest source package from the downloads page and extract it. Open a terminal, move to the nxt-python directory using `cd nxt-python` (or whatever directory you put it in), and run `python setup.py install`. To test the installation, run one of the programs in the examples directory.

### What if I already have LightBlue Glue installed with an older version of nxt-python or NXT\_Python? ###

If you have an existing [LightBlue Glue](http://www.cs.wlu.edu/~levy/software/nxt_lightblue_glue/) setup, you can update by doing the following:

Follow the instructions above. After doing that, remove your bluetooth.py directory (~/nxt\_lightblue\_glue in the setup example) and the PYTHONPATH line to that you added to your .profile or .bashrc.

Note that if you do not delete the bluetooth.py directory or PYTHONPATH enrty, NXT-Python _should_ still work fine, it will just use the external LightBlue Glue instead of the new internal one.

# Checking out the SVN to get the latest development version #
You will need to download an SVN client for your OS. For linux, the standard command line one works great. For Windows, I use TortoiseSVN. For Mac, I would suggest Google. After you get SVN working on your system, follow [Google's instructions](http://code.google.com/p/nxt-python/source/checkout) as far as the URL goes. Once you have a source tree, follow the installation instructions in the README (which basically amount to running `python setup.py install`).

# Then What? #

Now that you have successfully installed NXT\_python and one or more of the communication packages (PyUSB, PyBluez, or LightBlue), you can start using your nxt brick.

See [the tutorial page](http://code.google.com/p/nxt-python/wiki/Tutorial) or the examples in the examples folder for usage instructions.