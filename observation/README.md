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

### recoding NOAA data

good settings for recording NOAA data:
    
  * Demodulation: WFM (Wide FM, mono).
  * Filter Width/Bandwidth: Narrow is fine! (Custom, just wide enough so the signal fits as you can see on the spectrum analyzer.)
  * Alex says: "Start by tuning the FCD PLL to 23 kHz above or below the satellite frequency. For example, NOAA-18 transmits on 137.9125 MHz and I tuned the FCD to 137.935 MHz. Then tune the channel filter so that the RX frequency will correspond to the satellite frequency (see the Gqrx screenshot above). In the demodulator options select FM with 17k deviation and disable de-emphasis by setting the filter time constant tau to 0."
  * Filter Shape: “Normal” on GQRX or “Blackman-Harris 4” with order 250 on SDR#.
  * AGC (Automatic Gain Control): off.
  * LNA gain: Max, but you can play with it and guess where it has the best signal to noise ratio (i.e. where it sounds better).
  * No “Noise Blanker” or “Noise Reduction”.
  * Squelch: Disabled (-150dB).
  * No “DC remove”, “Correct I/Q”, “I/Q Balance”, “Swap I/Q”, etc.
  * Input rate: Around 1,000,000 (1.00Msps).
  * Decimation: None.

details on tuning and filter: https://oz9aec.net/radios/gnu-radio/howto-receive-and-decode-noaa-apt-images-with-the-funcube-dongle-and-gqrx

start recording when satellite pass windows starts, stop when it's over.

convert signal to 11205:

```
 sox input.wav output.wav rate 11025

```
or use audacity to resample.

audacity is also useful for cleaning, cutting, etc

## decoding data

decode the resulting 11025 kHz wav file either with


```
wxtoimg -t n /path/to/input.wav /path/to/output.png

```
or interactively with noaa-apt tool:
decode - process - save.

test data for a dry run: https://noaa-apt.mbernardi.com.ar/examples/argentina.wav

## what else?

  * making a remote SDR station with e.g. a raspberry pi and using rtl_tcp
     * gqrx source becomes "other", 'rtl_tcp=IP:port' 

## further reading

https://www.rtl-sdr.com/rtl-sdr-tutorial-receiving-noaa-weather-satellite-images/

https://www.rtl-sdr.com/simple-noaameteor-weather-satellite-antenna-137-mhz-v-dipole/

https://oz9aec.net/radios/gnu-radio/noaa-apt-reception-with-gqrx-and-rtlsdr

https://oz9aec.net/radios/gnu-radio/howto-receive-and-decode-noaa-apt-images-with-the-funcube-dongle-and-gqrx


