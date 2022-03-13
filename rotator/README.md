# Rotator workshop

In this activity we have 2 antenna rotators, one fully configured and one assembled but not calibrated.

We have some linux laptops pre-installed with gpredict and gqrx for experimentation.

Use gpredict to track satellites, set a ground station plan a pass observation.

## Overview
```
TLE -> Gpredict -> rotctld -> MD-01 -> spx rotator 
                -> rigctld -> GQRX -> SDR
                
```

Satellite positions are published using TLEs. 
Gpredict downloads these and uses the orbital elements and provides a handy user interface for planning passes. 
Gpredict produces  azimuth and elevation outputs that describe a satellites path across the sky at a particular observation location.
The AZ and EL are passed to rotctl software which provides an interface to the antenna rotator in a command language that the rotator understands.
Rotctl is installed on a small computer attached to the rotator controller.
The rotator controller directly controls the motors in the antenna rotator causing the antennas to track the satellite.

## Prediction Software

### Gpredict

http://gpredict.oz9aec.net/

"Gpredict is a real time satellite tracking and orbit prediction program
for the Linux desktop. It uses the SGP4/SDP4 propagation algorithms together
with NORAD two-line element sets (TLE).

Some core features of Gpredict include:

- Tracking of a large number of satellites only limited by the physical
  memory and processing power of the computer
- Display the tracking data in lists, maps, polar plots and any combination
  of these
- Have many modules open at the same either in a notebook or in their own
  windows. The modules can also run in full-screen mode
- You can use many ground stations
- Predict upcoming passes
- Gpredict can run in real-time, simulated real-time (fast forward and
  backward), and manual time control
- Detailed information both the real time and non-real time modes
- Doppler tuning of radios via Hamlib rigctld
- Antenna rotator control via Hamlib rotctld"

In this workshop we predominantly use gpredict for planning up coming passes and observations. This is because it is capable software that integrates well with hamlib, allowing the control of a wide range of rotors, and also is able to conrtol radios for doppler. We also use gpredict as our groundstation is entirely linux based.

Repository https://github.com/csete/gpredict
Manual https://github.com/csete/gpredict/raw/master/doc/um/gpredict-user-manual.odt

### N2YO 

https://www.n2yo.com

N2YO is a convenient website for quickly checking up to date satellite with a good option for easy 10 day look ahead.

### Orbitron

http://www.stoff.pl/

"Orbitron is a windows satellite tracking system for radio amateur and observing purposes. It's also used by weather professionals, satellite communication users, astronomers, UFO hobbyist and even astrologers.

Application shows the positions of satellites at any given moment (in real or simulated time). It's FREE (Cardware) and it's probably one of the easiest and most powerful satellite trackers, according to opinions of thousands of users from all over the world."

Orbitron is a popular windows alternative.

## Current satelites

http://celestrak.net/NORAD/elements/gp.php?GROUP=active&FORMAT=tle

Check celestrak for up to date TLE's for current satellites.

Set the TLE source location in gpredict and update the TLE's regularly before observations. TLE's can become inaccurate in as little as week for LEO satellites.

## Hamlib

https://hamlib.github.io/

"The Ham Radio Control Library–Hamlib, for short–is a project to provide programs with a consistent Application Programming Interface (API) for controlling the myriad of radios and rotators available to amateur radio and communications users."

Hamlib is a collection of command line applications for ham radio applications. For the rotator the most important is `rotctl` and its daemon (server) version `rotctld`

On some systems hamlib must be downloaded and compiled from source code especially if the most recent version is required as is the case for our 
rotator.
### Install
On ubuntu or debian based systems INSTALL is:

`sudo apt-get install libhamlib-utils`

otherwise for more recent versions you will have to install from source.

https://github.com/Hamlib/Hamlib/blob/master/INSTALL

### Usage

rotctl can be directly drived from the command line for a directly connected rotator over USB

`rotctl -m 903 /dev/ttyUSB0`

The 903 specifies the rotator model. `rotctl --list` for a list of supported rotators.

Then commands can be entered at the prompt.

```
P: set_pos     (Azimuth, Elevation)
p: get_pos     ()
S: stop        ()
M: move        (Direction, Speed)
```

Rotctl can also be set as a daemon on an ip address

`rotctld -m 603 -r /dev/ttyUSB0 -s 115200 -T 127.0.0.1`

Now rotctld will run in the background and can be controlled by an external software in this case gpredict.
'
In gpredict `edit>preferences>interfaces>rotators>Add New`
You can set up the rotator to communicate with rotctld. You will need to know the ip address of the computer with rotctl on it.

In our case we use a raspberry pi on 10.7.7.125 or similar.

The raspberry pi is connected directly to the rotator by USB.

Another approach is to use a rotator with an ethernet port. In this case roctld must serve over a virtual serial port using socat. This has not been found to be stable.

`socat -d -d -v pty,link=/home/julian/dev/tty0,raw tcp:10.7.7.125:23&`

### Multiple rotators on one server

https://www.la1k.no/2017/09/15/unified-software-rotor-control-over-the-local-network/

Multiple rotators can be served by multiple rotctld instances on one raspberry pi. The pi just needs to distinguish between the rotators somehow. This is done using udev rules following the schema above.

These are configured for 2 different ports and so gpredict can control them separately.


## Rotator controller

The rotator controller provides a control interface, and controls the motors in the actual rotator. In some cases the power supply electronics are also housed alongside the rotator controller as with the MD-01

### MD-01 

http://cdn.spid.net.pl/manuale/Manual-MD-01-EN.pdf

We use MD-01 rotator controllers which are supplied with the SPX and BIG RAS HR rotators. They are well tested and functional, however need to be upgraded to version 2 firmware to be compatible with rotctl.

The rotator is connected to the motors, and also gets feedback on the position of the rotator from rotary encoders built into the motor casing.

MD-01 requires configuration for each rotator, checking north azimuth, calibrating gearing ratios, checking 0 90 and 180 elevation are correct etc.

Most of this is done through the physical inteface.

### K3ng rotator

https://blog.radioartisan.com/yaesu-rotator-computer-serial-interface/

 DIY approach, K3NG that supports a wide range of motor and sensor types, including DC motors, steppers, compass accelerometer sensors, and rotary encoders, displays etc.

"This is an Arduino-based rotator interface that interfaces a computer to a rotator or rotator controller, emulating the Yaesu GS-232A/B, Easycom, and DCU-1 protocols which are supported by a myriad of logging, contest, and control programs.  It can be easily interfaced with commercial rotator control units.  "

K3NG is based on arduino, interfaces easily with rotctl through the yaesu 603 protocol, and is robust and well thought out. For someone wanting to build a low cost rotator controller from parts, this is a good option.

Most configuration is done in #define statements within the code.

We will not work with K3NG rotator in this workshop and it is included for completeness.

## Rotators

We are working with the following rotators.

https://www.rfhamdesign.com/

https://www.rfhamdesign.com/products/spx-antenna-rotators/spx-01-az--el/index.php

https://www.rfhamdesign.com/products/spid-hr-antenna-rotators/bigrashr/index.php

### Exercises

 - Choose satellites for observation in gpredict and create modules
 - simulate an observation pass
 - Steer the antenna directly with rotctl from the command line
 - Configre gpredict to operate different rotators
 - calibrate the new rotator for gearing, azimuth and elevation
