# Satellite Observation with SDR

  * hardware requirements
  * software requirements
  * target satellites
    * NOAA satellites
    * locating satellites
  * recording data with gqrx
  * decoding data

## hardware requirements

  * laptop or general PC with USB, Linux or Mac preferrable, but Windows possible
  * SDR usb dongle, e.g 
    * RTL-SDR Blog V3 R820T2 RTL2832U ($30 / DKK 300)
    * NOOELEC ($25 / DKK 250)
    * Airspy
    * Funcube
    * stronger / more expensive SDR such as HackRF One - SDR 
   * antenna - right hand circularly polarized antenna for 137 MHz
    
 ## software requirements
 
 ### gqrx 
 
 https://gqrx.dk/
 
 Gqrx is an open source software defined radio receiver (SDR) powered by the GNU Radio and the Qt graphical toolkit.
 By Alexandru Csete OZ9AEC.
 
 A recent version of Gqrx is probably already available through the official software channels of various Linux distributions as well as Macports and  Homebrew for Mac OS X.

Source code and the latest release is avalable on GitHub.

#### alternative on windows: SDR#

https://airspy.com/download/


### audio tools

#### sox

http://sox.sourceforge.net

#### audacity

https://www.audacityteam.org/

### APT weather satellite decoders

#### wxtoimg

https://wxtoimgrestored.xyz/

WXtoImg is a fully automated APT and WEFAX weather satellite (WXsat) decoder. The software supports recording, decoding, editing, and viewing on most versions of Windows, Linux, and Mac OS X.

Currently unmaintained, abandoned in 2018. Version 2.10.11 was the last STABLE release of the software.  Version 2.11.2 was the last beta release of the software.  WXtoImg is closed source software.  WXtoImg still runs on most modern operating systems.

#### noaa-apt

https://noaa-apt.mbernardi.com.ar/

#### alternatives

https://noaa-apt.mbernardi.com.ar/alternatives.html



## target satellites

A good first test target are the NOOA satellites, NOAA = National Oceanic and Atmospheric Administration.

Polar Operational Environmental Satellites (POES).

Automatic Picture Transmission (APT), 24/7

Active NOAA APT Satellites

    NOAA 15 on 137.6200 MHz FM
    NOAA 18 on 137.9125 MHz FM
    NOAA 19 on 137.1000 MHz FM

### locating the NOAA satellites

either via tools like wxtoimg or

NOAA 15 https://www.n2yo.com/satellite/?s=25338

NOAA 18 https://www.n2yo.com/satellite/?s=28654

NOAA 19 https://www.n2yo.com/satellite/?s=33591

## recording data with gqrx

### getting started with gqrx

get familiar with the GUI and settings by tuning in to some FM radio station, e.g. 100 MHz

good settings for recording NOAA data:
    
  * Demodulation: WFM (Wide FM, mono).
  * Filter Width/Bandwidth: Narrow is fine! (Custom, just wide enough so the signal fits as you can see on the spectrum analyzer.)
  * Filter Shape: “Normal” on GQRX or “Blackman-Harris 4” with order 250 on SDR#.
  * AGC (Automatic Gain Control): off.
  * LNA gain: Max, but you can play with it and guess where it has the best signal to noise ratio (i.e. where it sounds better).
  * No “Noise Blanker” or “Noise Reduction”.
  * Squelch: Disabled (-150dB).
  * No “DC remove”, “Correct I/Q”, “I/Q Balance”, “Swap I/Q”, etc.
  * Input rate: Around 1,000,000 (1.00Msps).
  * Decimation: None.



## further reading

https://www.rtl-sdr.com/rtl-sdr-tutorial-receiving-noaa-weather-satellite-images/


  * point
  * point

```
code

```