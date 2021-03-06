---
layout: default
title:  "Verification"
permalink: verification
---

#### &uarr;[top](https://ubiquityrobotics.github.io/learn/)

# Basic Verification Of Operation

This page tells how to verify basic operation of the Magni robot.  These tests can be used to regression test hardware and firmware changes or new production boards installed in the Magni robot.

There is a main board we call the ```Motor Control Board``` or ```MCB```. This is sometimes called Main Control Board as well.

There is a ```host linux``` computer which is the ```Raspberry Pi single board computer``` at this time and it plugs into the MCB as well as is powered from the MCB supplies.

If you have Magni Silver then the robot will have a ```Sonar Board``` which will have a few blue LEDs on top that will be helpful but not required to do these tests.

### Powering Up The Magni ROBOT

Before we get going on the tests a quick word on the two power switches on the ```Switch Board``` that plugs into the main board may be of value.  

Both switches must be in the OUT position for full robot operation.

* The black switch to the left as you look at the robot from the front is the ```Main Power``` button.  To enable this the switch must be out and not pressed in. When the main power is enabled the blue led on the switch board is on.
* The red switch to the right is the ```ESTOP Switch``` and it is easiest to think of this as the power for the motors.  To power up the wheel motors this switch must also be in the OUT position and the red led on the switch board will light when power is enabled.

As of later in 2019 switch boards started to have two connectors so that users could have their own switches on their product.   If you have a current switch board then there will be a white connector just behind the ESTOP switch to the right which MUST BE SHORTED OUT for motor power to work. We ship this version switch board with a jumper in that jack but it can get lost.

## Part 1:  MCB Hardware Level Verification

### 1.1 Power Supply And Status LED Indications
There is a line of 4 power supply indicator LEDs and a 'STAT' or status led that are in a row to the lower left of the board.  The 4 power supply leds should all be on and the status LED default state is to be on almost all the time but have very brief 'dropout'blink that form a blink every 4 to 6 seconds.  See the [firmware_upgrade page](https://learn.ubiquityrobotics.com/firmware-upgrade) for  the expected blink rates of released firmware.

Revision 5.0 main control boards and after have the leds in a horizontal line with the 'STAT' led to the right.  On version 4.9 and other early production units the LEDs are vertical with 'STAT' at the top of the line of leds.

If the 'STAT' led is off or does not blink there is something wrong with the onboard microprocessor subsystem on the main board.

Starting with version 5.2 of the main control board, MCB, there is an onboard 3.3V power supply and a blue led up near the top and a bit below the large white 'MAIN' 4-pin power jack.  This led has the label 3.3V and must be on.

### 1.2  The Electronic Circuit Breakers And Power Indicators
Starting with main board version 5.0 there are two LEDs that indicate the ECB circuit involved is active and passing power.  The switch board switches must be able to control the two ECB circuits as discussed below.

On the lower left of the main board a blue LED will be ON if the `Main ECB` has been enabled due to the main power switch being set to the out position. The main switch is black and is to the left on the little switch board.

On the lower right of the main board a blue LED will be ON if the `ESTOP` switch is set to the out position AND the main power switch is also in the out position.

### 1.3  ESTOP Switch Motor Power Safety Overrides

This test can be done without a raspberry Pi installed or with it installed.  

Start with the robot powered down and the 'ESTOP' switch in the 'out' position and the black main power switch pushed in so the Magni is totally off. Then push the black main power switch which will turn on main power. At this point the main power switch will be in the 'out' position.  Verify both the blue and red leds are lite on the switch board.

Verify that there is no jump in motors that move the robot more than one cm or so and that the motors are in the locked state strongly resisting movement.

If the Raspberry Pi is installek, wait for motor node to be fully started which takes up to 2 minutes but is often 30 seconds or so sometimes.  On rev 5.2 MCB you can see the motor node is active when both the 'SIN' and 'SOUT' leds near center top of the MCB are both blinking quickly.

Verify that the robot does not jump or move more than 1 cm as the motor_node has been engaged.  Verify that the Wheels PID locked still and no jump in movements.

Now push in the red ESTOP switch and verify the ESTOP led goes off on the switch board and also fades within 5 or so seconds on the lower right of the MCB board where the 'Motor Power' led is located.  Also verify the motors can be turned.  Note that some resistance is natural and expected.

Only do this next part of this test on rev 5.1 or later boards with v32 or later firmware.  While ESTOP is active and motor power is off turn both wheels by a half turn or so in either direction.   After the wheels have been turned press ESTOP so that the switch is OUT again.  There should be little to no movement in the wheels

## Part 2:  Basic Host To MCB Tests

### 2.1 Serial Communications LEDS
Starting with version 5.2 of the main control board, MCB, there are two leds that show if serial communications are active between the host processor (Raspberry Pi) and the MCB.   Both of these leds are located in the middle of the MCB and very high up near the white power jacks at the top of the PC board.

The lower blue LED is the SOUT led and will blink rapidly when the MCB processor is active even if there is no host processor.  This LED shows the signal seen by the host processor so the level shifter must work for this to be seen.

The upper blue LED is the SIN led and will blink when the host processor is actively sending commands and queries to the MCB.  This is the most important led to be blinking.   It indicates of the ROS node called /motor_node is actively controlling the MCB.  Note that depending on certain network conditions it may take up to a couple minutes for host motor_node to start communications with the MCB.

### 2.2 Wifi HotSpot Verification

If no LAN cable is attached and if the robot has not been configured to look for a WiFi OR if no WiFi can be seen then the Magni software will create a HotSpot that you can connect to with your laptop.

Do these tests with no Lan cable plugged into the Magni host computer.

If you have Magni Silver with the Sonar board then as the robot is powered up the LED2 (right LED on Sonar board as seen from the front) will light with dim blue light.  If you have a rev 5.2 MCB there will also be the WiFi led on the MCB but it goes off when the sonar board one goes on so they alternate.

After 6 or so seconds that LED on the sonar board will turn off. After about 16 or so seconds if WiFi is able to come up LED2 will start to blink brightly about once per second indicating that the WiFi HotSpot is up.  We are working on enhancements to be available by mid 2020 which will indicate AP mode active or that the wifi specified by pifi utility is not available and perhaps more states on this led.

Starting with main control board version 5.2 there will also be a wifi led on the right visible from the front of the magni.  This led is opposite from the sonar board led so it is off when the sonar board led is on and so on.

#### 2.2.1  Connect To The Magni Hotspot

At this time if you have on your smartphone some sort of WiFi network scanner you will see a ubiquityrobotics WiFi with last 4 digits being a unique hex value.

You will also see this HotSpot show up on your laptop and will be able to connect.  Read [HERE](https://learn.ubiquityrobotics.com/connecting) for more.

Verify you can connect to the Magni using password 'robotseverywhere' and verify you can open a console using SSH.

#### 2.2.2  Verify Magni Can Connect To Your Local Wifi

For this test you should follow the configuration page for ```pifi``` to configure your own network once you are connected using pifi commands available  [HERE](https://learn.ubiquityrobotics.com/connect_network) .

### 2.3 Check Operation Using The /diagnostics ROS topic

When the robot is running quite a few pieces of diagnostic information can be viewed by looking at the ROS topic the motor node publishes.  The diagnostic topic has other things besides the motor node so we filter it for firmware info in the example below for easy reading.

    rostopic echo /diagnostics | grep -A 1  'Firmware [DV]'

You can view the full diagnostics output and find other info such as:

  - `Battery Voltage` key shows the battery level in DC volts
  - `Motor Power` key is True when the ESTOP is enabling wheel power
  - `Firmware Version` shows the main board firmware version
  - `Firmware Date` shows the date for the firmware
  - `Firmware Options` shows hardware options if enabled

In the raw /diagnostics output these things may also be useful for feedback to factory on some issues.  For full output of /diagnostics do this.

    rostopic echo /diagnostics

### 2.4 I2C Bus Devices
The I2C bus on the host CPU needs to be able to communicate to a few devices on the MCB.  There is an I2C excpander at addr 0x20 and RealTime clock chip at address 0x6F. If there is a OLED display loaded on P2 it is at 0x3c. We should stop the motor node then run i2cdetect which is part of i2c-tools package.

    sudo systemctl stop magni-base.service
    sudo i2cdetect -y 1

The above command will output 8 lines each with 16 possible hex addresses. We want to note that it detected devices that are present on the I2C bus.  The OLED display was optional prior to MCB rev 5.2.   The table that follows shows likely addresses.

| | | |
|---|---|---|
|Device| I2C Address|Notes|
|SSD1306 OLED Display|0x3C|Shipped starting on MCB Rev 5.2|
|PCF8574|0x20|IO Expander|
|MCP7940 RT Clock|0x6f|RTC will show as UU addr|

* Because the MCP7940 is owned by the kernel the i2cdetect tool will show it at address 0x6F as  ```UU```.  This indicates it was seen.  If the kernel recognized the RTC properly there will be a  /dev/rtc0 device for final confirmation.

* If there is a production issue where a PCF8574A was incorrectly loaded then you would see address 0x38.  This is considered a misload in production.


After this test you may restart magni-base service

    sudo systemctl start magni-base.service


## Part 3:  Basic Movement Tests:

This set of tests has the focus of verification of circuits and commands that are related to robot movement or the ESTOP safety feature.

### 3.1  ESTOP Switch Motor Power Safety Override

This test starts out the same as the more complete test in Part 1 earlier in this doc so if you did that this test may be skipped as it is a subset.

With no WiFi connected, have the red 'ESTOP' switch in the 'out' position and the black main power switch pushed in so the Magni is totally off. Then push the black main power switch which will turn on main power. At this point the main power switch will be in the 'out' position.

Verify that there is no jump in motors that move the robot more than one cm or so and that the motors are in the locked state strongly resisting movement.

Wait for motor node to be fully started which takes 20 seconds or
so sometimes.  On rev 5.2 MCB you can see the motor node is active when both the 'SIN' and 'SOUT' leds near center top of the MCB are both blinking quickly.

Verify that the robot does not jump or move more than 1 cm as the motor_node has been engaged.  Verify that the Wheels PID locked still and no jump in movements.

### 3.2 Distance and Low Speed Movement Tests:

Enter keyboard movement using:

    rosrun teleop_twist_keyboard teleop_twist_keyboard.py  

  Press the  'z' key about 12 times until the 'speed' value shows about 0.15 meters per second;
    the 'turn' value will show about 0.3

  At this point the robot will not move because when teleop is first entered it is in same state as if the 'k' was hit.

We need a second window open that we will call the 'tf' window; in that window type:

Also as setup have a second window open and in that type:

    rosrun tf tf_echo odom base_link  

This command will continually update the robot position. There will be one line that shows the translation and 3 values that are for X,Y,Z in meters.  The line will look like this if the robot was powered up in it's current position:

    Translation: [0.000, 0.000, 0.100]   at first where X and Y are 0.000.

Now we will do a few tests so make sure the robot has room to move forward about 1 meter and could have room to rotate fully. Because these tests are not precisely timed the distances and rotations will be only near the expected values.

   - Put a piece of tape or a coin on the floor beside the central hub of a front wheel to know where the robot started.

   - Press  **k** key then use **z** or **q** until 'speed' value is near 0.15 MPS.

   - In the teleop window press  the **i** letter key at a quick rate for 4 seconds. This should move the Magni about 0.5 - 0.6 meters forward which is about one spin of each wheel since one rotation is very near 0.64 meters.

   - Look at the 'Translation' line in the second tf window and the first of the 3 numbers is X and verify with a ruler that the translation is very close to the measured distance on the floor.

   - In the teleop window press the **,** (comma) key at a quick rate for 4 seconds. This should move the Magni backwards to about where it started.

   - Look at the ‘Translation’ line in the second tf window and the first of the 3 numbers is X and should have returned to near 0.0. Because of human timing there will be a small error.

   - Next press the **j** letter key at a quick rate for about 5 seconds so the Magni rotates a little more than 90 degrees to face left. The tf window will have the 3rd line that says 'degrees' where at this rotation the 3rd number should be near or just above 90 degrees to the left. If it goes too far you can use quick taps to the l key to inch it back to about 90.

   - Press the **l** letter key at a quick rate for 5 seconds and the Magni will rotate clockwise back to the starting point and will have the 3rd line that says 'degrees' now show the 3rd number to be near 0.

Another mode you may want to try is while the robot is up on blocks so drive wheels don't touch the ground you can get wheels to run a fixed speed as follows:

    rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{ linear: {x: 0.3} }'  

If you want the robot to continuously rotate this can be done directly on the ground or on blocks as follows:

    rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{ angular: {z: 0.5} }'




### 3.3 ESTOP Testing In A Running Full System

There have been other ESTOP tests that appear earlier in Part 1.  The reason for these tests is to validate ESTOP operation when the host has been sending movement command to the MCB.

NOTE:  These tests require a rev 5.0 or later MCB board.
YOU MUST NOT DO THIS TEST ON REV 4.9 boards as it will be dangerous!

Be sure the motor node is running which can be easily seen with a rev 5.2 or later MCB by inspection that both the 'SIN' and 'SOUT' leds in top center of the MCB board are blinking quickly.

Place the robot on blocks so it does not 'get away from you' for these tests.

#### 3.3.1  Entering The ESTOP state with Movement In Progress

Run the joystick or use 'twist' to make motors actively move.  Press ESTOP to active state which will cause the 'Motor Power' led to turn off in the lower right of the MCB.

Verify the motors will no longer have power and will slow to a stop
with mild 'self braking' resistance to movement.

#### 3.3.2  Exiting The ESTOP state with Movement Commands

With the robot already in ESTOP state so there is no motor power, run the joystick or the twist program actively for a couple seconds and while doing so release the
 ESTOP while still issuing movement commands.

Verify that the robot will start moving at the speed it is being commanded in a controlled way and not at full speed or other speed.

No matter how many movement commands were issued when ESTOP is active, it is only on the release of ESTOP  after a half second or so that that velocity will be re-enabled as the wheels nicely ramp to speed again.

### 3.4  Speed Tests:

The speed tests verify proper operation of speed regulation and limits. These tests are done from the command line without the teleop twist program so use Control-C if you have the teleop twist program running.

These tests should be done with the robot on 'blocks' for the big powered front wheels so the wheels do not touch the floor. Normally we put a block of wood or a small stack of books under the front of the robot and it raises it up so the wheels do not touch the floor.

Put a piece of tape on the outside of a wheel so while testing we can count revolutions to get the actual speed.

#### 3.4.1  Medium Speed Test:

Here we look to verify a medium speed is correctly regulated for both forward and backward driving.  This test is done from the command line without the teleop twist program so use Control-C if you have the teleop twist program running.

YOU MUST HAVE THE ROBOT DRIVE WHEELS ELEVATED TO NOT TOUCH THE GROUND FOR THIS TEST.

Type the following command and hit enter so the robot moves at 0.3 meter per seconds

   - Type this: ``rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{ linear: {x: 0.3} }'``

   - Verify the speed is about 0.3 meter per second by watching that the wheels both turn one full revolution forward in about 2 seconds.  Use Control-C to stop movement.

   - Type this: ``rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{ linear: {x: -0.3} }'``

   - Verify the speed is about 0.3 meter per second reverse and that the wheels both turn one full revolution forward in about 2 seconds.  Use Control-C to stop movement.


### 3.4.2  Maximum Speed Limit Test:

We will drive at the max speed limit value will cause the robot to not exceed the default 1 meter per second setting.  This test is done from the command line without the teleop twist program so use Control-C if you have the teleop twist program running.

This test is best done using a stopwatch and timing how long 10 wheel revolutions takes.

YOU MUST HAVE THE ROBOT DRIVE WHEELS ELEVATED TO NOT TOUCH THE GROUND FOR THIS TEST.

Type the following command and hit enter so the wheels rotate at 1 meter per second

   - Type this: ``rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{ linear: {x: 1.0} }'``

   - Verify the speed is about 1 meter per second by watching that 10 turns of the wheel take about 6 seconds.  The circumference of the wheels is near 0.64 meters.  Use Control-C to stop the movement.

   - Type this: ``rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{ linear: {x: -1.0} }'``

   - Verify the speed is about 1 meter per second in reverse by watching that 10 turns of the wheel take about 6 seconds.  The circumference of the wheels is near 0.64 meters.  Use Control-C to stop the movement.

### 3.5 Deadman Timer Testing:

The robot is designed to return to zero speed if
it loses touch with constant host velocity commands.
   - Startup and run the robot on blocks at a constant speed. Kill
the motor node OR disconnect serial (if your system allows).
     The motor node can be stopped and starting using:

     `sudo systemctl stop magni-base.service`

RESULT: The robot will return to stopped state with wheels actively locked.
   - Start or re-connect serial to the motor node using

   `sudo systemctl start  magni-base.service`

RESULT: Robot should be operational after the motor node starts (takes 15 or more seconds to start).

For re-connect of serial it will start back up in a second or less.

## Part 4:  RaspiCam and Sonar Board tests

### 4.1 RaspiCam Camera Test:

There is a very simple way to test the RaspiCam camera on the robot.   This test will generate a jpeg still picture in about 6 seconds just to check the camera functionality.

    raspistill -o testpicture.jpg

To verify the camera is operating properly the testpicture.jpg file needs to be moved to your laptop or other computer that has a jpeg picture viewer.  If it is too difficult to move the picture using ftp or some other linux operation, the next best thing is to look at the file size. This can be done in the line below and the reply shows the 1543213 as the size in bytes for the jpeg image file.

    ls -l testpicture.jpg
    -rw-rw-r-- 1 ubuntu ubuntu 1543213 Aug  4 08:18 testpicture.jpg

If you have previously setup a laptop to be running ROS and have your Magni setup to be the ROS_MASTER you can start the raspicam node on the magni

    roslaunch raspicam_node camerav2_1280x960.launch

Then on the laptop which has graphics you can view live video like this:

    rosrun image_view image_view image:=/raspicam_node/image _image_transport:=compressed  

### 4.2 Sonar Board Test:

If you have installed and enabled the sonar board using the install guide viewed [HERE](https://https://learn.ubiquityrobotics.com/camera_sensors) then you can verify sonar operation in realtime once the robot has been started.


[The sonar node](https://github.com/UbiquityRobotics/ubiquity_sonar) publishes a `sensor_msgs/Range message` for each sonar reading.  Using `rostopic echo /sonars` you can view all the sensor readings in one topic where the frame_id of `sonar_3` would be for the front facing sonar 3.  Using the table that will follow you can place boxes in front of sensors to gain confidence that each sensor is showing the distance to that object.  A thin bar may not be seen properly and you may get mixed messages for what is behind it or may see the bar so use large objects for this test.

There is one separate topic for each sensor as seen in the table that follows. This table also has the jack number and 50-pin connector pins for echo and trigger  


| | | | | |
|---|---|---|---|---|
|Topic|     Direction| Jack | Trig Pin | Echo Pin  |
|/pi_sonar/sonar_0|   Far right| J5 | 38 | 40 |
|/pi_sonar/sonar_1|   45 degrees left| J1 | 32 | 36 |
|/pi_sonar/sonar_2|   45 degrees right| J6 | 16 | 18 |
|/pi_sonar/sonar_3|   Front| J4 | 13 | 15 |
|/pi_sonar/sonar_4|   Far left| J2 | 35 | 37 |

Rviz can visualize these messages as cones.  There are launch files to do this in:  
https://github.com/UbiquityRobotics/magni_robot (the source package, not the binary packages)

The [move_basic node](http://wiki.ros.org/move_basic) uses the messages published by the sonar node to determine proximity to obstacles.

## Part 5: Built In MCB Selftest

The robot is able to test some of it's own subsystems.  This ability is most capable starting with MCB 5.2 and will be first introduced in firmware version v36.

This section has been broken out from basic MCB tests due to it's complexity merits it's own section.

The results of the selftest will be available as status bits in a register of the MCB board and many of the tests have the ability to show up as blink codes on the `status`  led should the error be detected.

The blink codes will be shown by the status led goes dark for a half second then a series of 2 to 4 long and short blinks occur followed by another half second of darkness before normal blinking returns.  If a selftest fails with more than one error only the one considered the most important is visually seen on the LED.

Here is the table of blink codes

|  Blink Code | Error Description |
|-------------------------|----------------------|
|  Long Short Short Short |  Low Battery voltage |
|  Short Long Long Short  |  5V main or 12V main power error |
|  Long Short Short Long  |  5V Aux or 12V Aux power error |
|  Long Long              |  Motor Test Failed  |

### Default Power on Selftest

There will be a simple set of checks to look at the power supply levels every time the robot is started for any MCB of version 5.2 or later.  If any of the 5V or 12V power supplies are not functional an error will result as a one of the voltage blink codes seen in the table above.  

If the main battery is getting low an error will result but the robot will be allowed to start.  The main battery test will work on all version of the MCB.  ```The battery low condition shows up as a long blink followed by 3 shorter blinks```.  This blink code also will happen every 45 seconds so watch for this to remind you battery is very low which leads to erratic operation.

### The Motor And Wheel Encoder Test
A test of the two drive wheels can be enabled by connecting TP4 to ground as the MCB is powered on.  On MCB rev 5.3 and beyond this is easily done by using a standard 0.1" spacing jumper placed on the back of the board seen from battery compartment.  ```Place the jumper between the bottom 2 pins on the TP4 side on the 3-pin P706 header located near the center on the back of board then power up the robot```.

The drive test is testing wheel sensitivity as well and to run properly the robot front wheels should not be on the ground so we suggest a block of wood or other object be placed under the front of the robot so the wheels do not touch the ground when this test is run.  Both wheels will turn a small amount one way and then another way in this short 8 second test.

Normally when this works the status led will simply drop into it's normal single dropout blink every 5 seconds or so.  ```If you see a series of two long blinks that indicates the motor test failed```.

You can force this test to fail by holding back the wheels with reasonable force as this test runs which will force the blink code of  ```Long Long```

### The Runtime Battery Low TEST
Every 45 seconds as the robot runs the main battery will be checked and if the battery supply drops to 22.2 volts a low battery condition will be detected and show up as an LED blink code of  ```Long Short Short Short```
The threshold for this test is settable in firmware but the feature to set it using the host has not been implemented as of April 2020.

### Simulate Failure On Power Supply Tests

You can force the main power test fail code by connecting the TP4 testpoint to ground through a 2.2k or 4.7k ohm resistor as the test runs on power up.

You can force the aux power test to fail by connecting the TP3 testpoint to ground through a 2.2k or 4.7k ohm resistor.

### Programatically Running Selected Parts Of the selftest
Support for selection of one or more of the selftests to be run is done through some new registers in the firmware. Register with hex address 0x3b can request tests and register with hex address 0x3c will report results.   

In order to run these tests the main host code must be stopped first using ```sudo systemctl stop magni-base``` and then a new version of a python test tool in the ubiquity_motor repository in the scripts folder which is called ```test_motor_board.py```.  This script is not distributed yet but once available can be used to run 1 or more tests and report the results.  More will appear here once it is supported.

We hope to support tests in the future through more standard ROS mechanisms.
