Note: This guide is adapted from an email sent to me (marcusw) by rhn, who designed and wrote most of the new sensor code. I have wikified it and changed a few things.

Let's say you have a sensor which allows you to read a signed integer temperature value stored in 2 bytes in little-endian order at address `0x42`.

# Subclassing `BaseDigitalSensor` #
`BaseDigitalSensor` manages setting up the digital sensor and massively helps reading values from it. There are three things it requires to work: setting up static variables, initialization and creating a method for getting the value.

If you want to initialize something yourself, then create an `__init__` method and use `super(TemperatureSensor, self).__init__()`. If you don't need to run code or write values automatically on startup, the init() method can be left out.

To read or write i²c values in a convenient manner, you can use the `I2C_ADDRESS` dictionary. It tells `BaseDigitalSensor` how to find values on the i²c bus. The format is: `{name: (address, type)}`. Name is a string you want to use, address is location of the first byte on i²c bus, and type is a string telling how to read the data, as interpreted using `struct.unpack()` or `struct.pack()`. See [the `struct` docs](http://docs.python.org/library/struct.html#format-strings) for more info.
The dictionary MUST BE COPIED BEFORE USING! It's a static variable of `BaseDigitalSensor`, which means if you modify it somewhere, it gets changed everywhere. If you copy it with:
```
    I2C_ADDRESS = BaseDigitalSensor.I2C_ADDRESS.copy()
```

then you "cover" the old one with the copy (in this class's namespace).

For this example, we need an entry for the temperature value:
```
    I2C_ADDRESS.update({'temperature': (0x42, '<h')})
```
The only non-self-explanatory bit here is the `'<h'`, which if you look at the struct page you'll see to mean "a signed two-byte integer in little-endian format". If you need more than one address, simply add more keys and values. Note that an address can have more than one name and way of reading it (helpful for sensors with multiple modes).

If the sensor uses a different device address than the standard 0x02, you will need to include that. Here is how to do it:
```
    I2C_DEV = 0x31
```
If you don't know the device address of your sensor, it's probably 0x02 and will work fine by default. If you are interfacing to an i²c-compatible chip which is not designed to work with the NXT, the device address is multiplied by two for this application. That is, 0x01 would be 0x02, 0x02 would be 0x04, and so on.

There should be at least two methods for getting the value. One is called `get_sample()` and it should do "the right thing". In case of a temperature sensor, return the temperature. If it's a sound sensor, return sound value; if the sensor measures acceleration, then return the measured acceleration (notice that Accelerometer class returns a class called Acceleration...this is a good idea for sensors with more than one value per sample).

The rest of the methods should be called `get_X`, where `X` is the actual name of what you want to read. This comes in handy when the sensor can read multiple different things, and it makes the programmer more aware of what he/she is really doing. The `get_sample()` method should point to one of these. In our case, it's `get_temperature()`.

Now that you've set up the name to address dictionary, you can forget about i²c and read the value with `self.read_value()`. It takes the name you entered previously and returns what `struct.unpack()` returned. NOTE: `struct.unpack()` always returns a tuple, even if you read only one value:
```
   def get_temperature(self):
        """Returns temperature in Celsius."""
        # return self.read_value('temperature') # WRONG! returns (23,)
        return self.read_value('temperature')[0] # correct, returns 23
```

Congratulations! You have a working sensor. It should be something like:
```
class TemperatureSensor(BaseDigitalSensor):
    I2C_ADDRESS = BaseDigitalSensor.I2C_ADDRESS.copy()
    I2C_ADDRESS.update({'temperature': (0x66, '>h')})
    def __init__(self, brick, port, check_compatible=False):
        if check_compatible:
            print "sorry, it's not supported yet"
        super(TemperatureSensor, self).__init__(beick, port, False)

    def get_temperature(self):
        """Returns temperature in Celsius."""
        # return self.read_value('temperature') # WRONG! returns (23,)
        return self.read_value('temperature')[0] # correct, returns 23
    
    get_sample = self.get_temperature
```

# Modes and writing #
Suppose the sensor can present the temperature in two modes, and uses another i²c address to do it. It's a single byte at address `0x41`, which can take the values of `0x1` for Celsius, `0x2` for Fahrenheit and `0x3` for Kelvin (in which case, the value would be unsigned because kelvin values are never below zero). After writing to the field, the value of temperature address changes. You can also read the mode at any moment.

The first necessary thing is adding more entries in `I2C_ADDRESS`:
```
    I2C_ADDRESS.update({'temperature': (0x42, '<h')})
```
becomes
```
    I2C_ADDRESS.update({'temperature': (0x42, '<h')},
                       {'tempkelvin': (0x42, '<H')},
                       {'mode': (0x41, 'B')})
```
As you can see, there is an added 'mode' key and an extra name for reading the temperature when in kelvin mode (which would merit an additional get\_temp\_kelvin() method).

Next, adding a method to read the mode:
```
    def get_mode(self):
        """Returns the mode."""
        return self.read_value('mode')[0]
```

Finally, setting the mode by writing to `0x41`. It's achieved in a similar way as reading. Function called `write_value()` takes the name of the field and the values tuple. Warning: it's a tuple again, even with one single argument. The code:
```
    def command(self, mode):
#        self.write_value('mode', mode) # Nay.
        self.write_value('mode', (mode, )) # Yea. One-element tuple.
```

Don't hesitate to add enumerations:
```
    class Commands:
        CELSIUS = 0x1
        FAHRENHEIT = 0x2
        KELVIN = 0x3
```

The user can do:
```
>>> thermometer.command(thermometer.Commands.Celsius)
```
and it will work well. Passing string arguments makes `command()` unnecessarily complex and prevents IDEs from completing your syntax.

# Checking for sensor compatibility #
The new version of sensors module can prevent you from plugging sensors in incorrectly! It's being done by checking the identification data of a sensor and comparing it to what the sensor class is compatible with. This feature is enabled by default and will print a warning message if a sensor class is used with the wrong hardware.

This feature is what `check_compatible()` was for. Creating a digital sensor on a port with a different digital sensor connected will raise an error.

To make use of it with our temperature sensor, we need to check its identification data and pass it to `BaseDigitalSensor`. Fortunately, all sensors have the same addresses for this, and the code is already in `BaseDigitalSensor`. Just call:
```
>>> sensor_info = thermometer.get_sensor_info()
>>> print sensor_info
Version: `�V1.23  `
0xfd, 0x56, 0x31, 0x2e, 0x32, 0x33, 0x20, 0x20, 
Product ID: `HiTechnc`
0x48, 0x69, 0x54, 0x65, 0x63, 0x68, 0x6e, 0x63, 
Type: `Compass `
0x43, 0x6f, 0x6d, 0x70, 0x61, 0x73, 0x73, 0x20, 

```
(here we use the data from the HiTechnic Compass sensor as an example...if we were really using a temperature sensor, the output obviously would be different)

The hex numbers below each field string are provided in case of binary chars in the strings, seen as the "�" in the first string. Looking at the hex, we can see that the corresponding number is `0xfd`, extended ASCII for ². In a database entry, this would be represented as `'\xfd'`.

After you have the data, add the following line immediately after the class declaration:
```
TemperatureSensor.add_compatible_sensor(None, 'HiTechnc', 'Compass ')
```

NOTE: this function is smart enough to ignore trailing `'\x00'` but include all other characters in the call. You can also call it multiple times to indicate that sensors with other identification data are compatible.

NOTE: If you put `None` instead of one of more the arguments, the system will assume the class is compatible regardless of its value. This works well for classes with which any `Version` of a sensor is compatible.