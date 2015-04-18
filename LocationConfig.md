While passing many arguments to find\_one\_brick() detailing your NXT's location and properties works, it's not an optimal solution. As of v2.2.0, NXT-Python has an alternative way of configuring brick location information, that is, a configuration file. This method has the advantage that once it's set up, you can connect to your brick no matter what its location by simply using find\_one\_brick() with no arguments (except perhaps debug or silent). To get started, run these in a python prompt:

```
import nxt.locator
nxt.locator.make_config()
```

This will create an example configuration in the correct location which you will need to edit to make it work with your brick. Here it is:

```
[Brick]
name = MyNXT
host = 54:32:59:92:F9:39
strict = 0
method = usb=True, bluetooth=False, fantomusb=True
```

This file would be translated into something like this by find\_one\_brick():

```
nxt.locator.find_one_brick(name='MyNXT', host='54:32:59:92:F9:39', strict=False, method=Method(usb=True, bluetooth=False, fantomusb=True))
```

You should change all the options here to match your brick. While the host parameter is optional (as are the rest), it is a good idea to use it because it uniquely identifies your brick from any other device. If you don't need to use some of these options, you should remove them from the file.