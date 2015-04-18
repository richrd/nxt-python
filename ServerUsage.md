To run the server, use
```
import nxt.server
nxt.server.serve_forever()
```
or just double click on server.py in your site-packages/nxt directory.

If you want to use password protection, do something like
```
import nxt.server
password = 'nxtpass'
authorizedips = ['127.0.0.1', '125.76.12.98'] #these are the ips that don't need to send the password before any commands are carried out. Usually, this is left blank.
nxt.server.serve_forever(password, authorizedips) #both arguments are optional, but authorizedips won't have any effect if you don't provide a password.
```
Or you can pass a password (no pun intended) on the command line when starting the script, like `python server.py nxtpass`.

This is an example of a python script that uses the server (but really, it could be interfaced to in any language that supports UDP datagram sockets):
```
#This script is an example for the nxt.server module. You need to run
#nxt.server.serve_forever() in another window. Or, if you want to use
#this across a network, pass the IP of the computer running the server
#as an argument in the command line.

import socket, sys

try:
    server = sys.argv[1]
    bindto = ''
except:
    server = 'localhost'
    bindto = 'localhost'

insock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
outsock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
insock.bind((bindto, 54374))

while 1:
    command = raw_input('nxt> ')
    outsock.sendto(command, (server, 54174))
    retvals, addr = insock.recvfrom(1024)
    retcode = retvals[0]
    retmsg = retvals[1:len(retvals)]
    print 'Return code: '+retcode
    print 'Return message: '+retmsg
```
This will bring up a prompt (`nxt> `). Enter one of the commands in the table below and it will be sent to the server for processing.

When the server is running, it waits on port 54174 for incoming UDP datagrams. These datagrams are limited to a length of 100 bytes (or characters) in length by the server. To have the server execute a command, just send it in a datagram to that port. These are the available commands:
| **Name** | **Effect** | **Syntax Example** |
|:---------|:-----------|:-------------------|
| find\_brick | Looks for a brick and connects to it if it can | `'find_brick'` |
| get\_touch\_sample | Gets a sample from the port specified | `'get_touch_sample:1'` |
| get\_sound\_sample | Gets a sample from the port specified | `'get_sound_sample:1'` |
| get\_light\_sample | Gets a sample from the port specified, with option for illumination | `'get_light_sample:1,0'` |
| get\_ultrasonic\_sample | Gets a sample from the port specified | `'get_ultrasonic_sample:1'` |
| get\_accelerometer\_sample | Gets a sample from the port specified | `'get_accelerometer_sample:1'` |
| get\_compass\_sample | Gets a sample from the port specified | `'get_compass_sample:1'` |
| run\_motor | Starts a motor with the port, power, and regulation (bool) specified | `'run_motor:a,100,0'` |
| stop\_motor | Stops a motor with the port and braking (bool) specified | `'run_motor:a,1'` |
| update\_motor | Updates motor(s) with the port(s)<sup>†</sup>, power, and tacholimit specified | `'update_motor:a,100,300'` |
| play\_tone | Plays a tone of the frequency and duration specified | `'play_tone:500,100'` |
| close\_brick | Closes the connection to the brick | `'close_brick'` |

<sup>†</sup>You can specify more than one port for the update\_motor command by using the syntax of one or more ports encased in parentheses and separated with semicolons. For example: `update_motor: (a; b), 100, 100`

For each command, the server sends back a datagram on port 54374 in the syntax of errorcode (0 is success, 1 is failure), then a message. For sensors, the success message is the sample from the sensor. All command failure messages are gotten straight from the error traceback (using `sys.exc_info()`).

Notes:
  * It's all in UDP datagram sockets.
  * You will probably get an error if you do anything (besides entering a correct password, if one is required) before you do a `'find_brick'`.
  * If you get a failure code and a return message of "need more than x values to unpack.", it's because you forgot a parameter (or two or three) in a command that requires them.
  * Because of the universal nature of sockets, you can use this driver in just about any programming language that supports (datagram) sockets. Use your imagination!
  * If you have any questions or suggestions, post a comment below or contact me.