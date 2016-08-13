# STC DIY Clock Kit firmware

Firmware replacement for STC15F mcu-based DIY Clock Kit (available from banggood [see below for link], aliexpress, et al.) Uses [sdcc](http://sdcc.sf.net) to build and [stcgal](https://github.com/grigorig/stcgal) to flash firmware on to STC15F204EA series microcontroller.

![Image of Banggood SKU972289](http://img.banggood.com/thumb/large/2014/xiemeijuan/03/SKU203096/A3.jpg?p=WX0407753399201409DA)

[link to Banggood product page for SKU 972289](http://www.banggood.com/DIY-4-Digit-LED-Electronic-Clock-Kit-Temperature-Light-Control-Version-p-972289.html?p=WX0407753399201409DA)

## features
Basic functionality is working:
* time display/set (12/24 hour modes)
* date display/set
* display auto-dim
* temperature display in C

**note this project in development and a work-in-progress**
*Pull requests are welcome.*

## TODOs
* temperature display in C/F selectable (either build or run-time)
* alarm and chime functionality
* possibly: make 12/24 hr a build option (to save precious code space)

## hardware

* DIY LED Clock kit, based on STC15F204EA and DS1302, e.g. [Banggood SKU 972289](http://www.banggood.com/DIY-4-Digit-LED-Electronic-Clock-Kit-Temperature-Light-Control-Version-p-972289.html?p=WX0407753399201409DA)
* connected to PC via cheap USB-UART adapter, e.g. CP2102, CH340G. [Banggood: CP2102 USB-UART adapter](http://www.banggood.com/CJMCU-CP2102-USB-To-TTLSerial-Module-UART-STC-Downloader-p-970993.html?p=WX0407753399201409DA)

## requirements
* linux or mac (windows untested, but should work)
* sdcc installed and in the path (recommend sdcc >= 3.5.0)
* stcgal (or optionally stc-isp). Note you can either do "git clone --recursive ..." when you check this repo out, or do "git submodule update --init --recursive" in order to fetch stcgal.

## usage
```
make clean
make
make flash
```

## options
* override default serial port:
`STCGALPORT=/dev/ttyUSB0 make flash`

* add other options:
`STCGALOPTS="-l 9600 -b 9600" make flash`

## pre-compiled binaries
If you like, you can try pre-compiled binaries here:
https://github.com/zerog2k/stc_diyclock/releases

## use STC-ISP flash tool
Instead of stcgal, you could alternatively use the official stc-isp tool, e.g stc-isp-15xx-v6.85I.exe, to flash.
A windows app, but also works fine for me under mac and linux with wine.

**note** due to optimizations that make use of "eeprom" section for holding lookup tables, if you are using 4k flash model mcu AND if using stc-isp tool, you must flash main.hex (as code file) and eeprom.hex (as eeprom file). (Ignore stc-isp warning about exceeding space when loading code file.)
To generate eeprom.hex, run:
```
make eeprom
```

## clock assumptions
Some of the code assumes 11.0592 MHz internal RC system clock (set by stc-isp or stcgal).
For example, delay routines would need to be adjusted if this is different.

## disclaimers
This code is provided as-is, with NO guarantees or liabilities.
As the original firmware loaded on an STC MCU cannot be downloaded or backed up, it cannot be restored. If you are not comfortable with experimenting, I suggest obtaining another blank STC MCU and using this to test, so that you can move back to original firmware, if desired.

### references
http://www.stcmcu.com (mostly in Chinese)

stc15f204ea english datasheet:
http://www.stcmcu.com/datasheet/stc/stc-ad-pdf/stc15f204ea-series-english.pdf

sdcc user guide:
http://sdcc.sourceforge.net/doc/sdccman.pdf

some examples with NRF24L01+ board:
http://jjmz.free.fr/?tag=stc15l204

Maxim DS1302 datasheet:
http://datasheets.maximintegrated.com/en/ds/DS1302.pdf

VE3LNY's adaptation of this hardware to AVR (he has some interesting AVR projects there):
http://www.qsl.net/v/ve3lny/travel_clock.html

[original firmware operation flow state diagram](docs/DIY_LED_Clock_operation_original.png)
[kit instructions w/ schematic](docs/DIY_LED_Clock.png)


### chat
[![Join the chat at https://gitter.im/zerog2k/stc_diyclock](https://badges.gitter.im/zerog2k/stc_diyclock.svg)](https://gitter.im/zerog2k/stc_diyclock?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

