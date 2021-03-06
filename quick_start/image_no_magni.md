---
layout: default
title:  "Using Our Raspberry Pi Image Without A Magni"
permalink: image_no_magni
---

#### &uarr;[top](https://ubiquityrobotics.github.io/learn/)

## Using Our Raspberry Pi Image Without A Magni

### About Our Raspberry Pi Images

Ubiquity Robotics makes an image for the Raspberry Pi that it
shares freely. This image is meant to support the Magni
robot that we offer and we are not able to give much
assistance to those who don't have a Magni but who wish to use the image for their own projects.
We do offer some documents that
users of the image may find helpful and so this page is
meant to offer some assistance of the most likely questions
people may have about the image.


### Connecting To Your Network
Our image comes up and starts up a WiFi hotspot by default.
To learn more about how to connect with the hotspot, see
[**THIS_PAGE**](<https://learn.ubiquityrobotics.com/connect_network>).

You can also directly hook up a LAN cable to a router you may have that offers a DHCP connection.  If you do that you can find software to scan your network for the new IP address and then use a tool such as  ssh  or  putty to connect a console.

Also Note that pi3-miniuart-bt is enabled by default, so bluetooth stability may be affected. Disable it if you are using bluetooth but not the serial port.

### Disabling the Magni Support Software
The Ubiquity Robotics images will come up and run
the software required for a Magni robot by default.

To disable the Magni software you can use this line
once you connect with a linux console as discussed above.

    sudo systemctl disable magni-base

### The GUI

The GUI is provided for debugging purposes only. The primary method of using this image is headless via SSH. Minor GUI issues probably exist, but we don't have the resources to address them.

If you are trying to connect to Wifi from the GUI you must first disconnect from the ubiquityrobotXXXX network

### Disabling The Automatic Startup of roscore
The Ubiquity Robotics images will start up roscore by default.
Robot Operating System, ROS, uses a core process and the Magni software platform is based on ROS.

In some cases users may want to disable even the startup of roscore and you can use the line that follows to do so after connecting to a linux console.

    sudo systemctl disable roscore


### Using The GPIO Lines
Our image uses some of the GPIO lines to control our
Magni robot. By default many of the lines are
unused and these are the best ones to think about
using for your own uses. You should see the section
regarding [GPIO Lines Used For The Sonar Board](<https://learn.ubiquityrobotics.com/GPIO_lines>) and
use those lines as we do not use them until they are
enabled by users who order a Magni with a Sonar board.

### Using The I2C Bus
If you want to use the I2C bus be aware that
we expect and look for certain I2C devices. You should avoid
use of the I2C addresses that we use.  Also note that many
users don't realize they must supply pullup resistors to 3.3 volts
for proper I2C operation.  As we cannot know what you have
in mind a general rule is to try something on the order of
a 3.3 kiloohm resistor.  Note that many boards for I2C
devices may have the pullup resistors already on the
little sensor boards so just be aware you must think about
the pullup resistors in general.

Please see the addresses on I2C that we use and avoid
these addresses if you can as you may get our software
interacting with your software in some cases. See our
I2C discussion that is specific to Magni but will list
the addresses used in the section titled  Guidelines For
Usage Of The I2C Bus on [**THIS_PAGE**](<https://learn.ubiquityrobotics.com/diagnostics>).
