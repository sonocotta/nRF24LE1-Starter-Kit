# nRF24LE1 Starter Kit

![Open Source Hardware](/Images/open-source-hardware-logo.png)
![Open Source Software](/Images/open-source-software-logo.png)

Resources you need to start developing for nRF24LE1

## What is nRF24LE1

nRF24LE1 is a nRF24L01 radio module and 8051-core microcontroller in single chip. You can think of it as Arduino with nRF24L01 in a bundle. 

![General architecture](https://github.com/anabolyc/nRF24LE1_Starter_Kit/blob/main/Images/image000.png?raw=true)

It has all capabilities of nRF24L01 radio chip and quite powerfull MCU that can run your code.

## Why nRF24LE1

Just for fun. Also it is dead cheap. $2-3 for ready to use module. But it is outdated, not recomended for new design, and probably you should use nRF52X or nRF53X series chips instead.

![NRF24LE1 module](https://github.com/anabolyc/nRF24LE1_Starter_Kit/blob/main/Images/image004.png?raw=true)

But if you're still convinced, below steps will allow you to get started.

## Disclaimer

All below steps are tested on Ubuntu linux, and only that.

## Download and build SDK

First pull all the submodules.

```
$ git submodule update --init
```

Get to SDK folder and build SDK

```
$ cd SDK
$ make
```

```
[...]

Finished building target '_target_sdcc_nrf24le1_24' for library collection 'SDK'
make[1]: Leaving directory '/home/dronische/dev/__my/nRF24LE1_Starter_Kit/SDK/_target_sdcc_nrf24le1_24'

[...]

Finished building target '_target_sdcc_nrf24le1_32' for library collection 'SDK'
make[1]: Leaving directory '/home/dronische/dev/__my/nRF24LE1_Starter_Kit/SDK/_target_sdcc_nrf24le1_32'

[...]

Finished building target '_target_sdcc_nrf24le1_48' for library collection 'SDK'
make[1]: Leaving directory '/home/dronische/dev/__my/nRF24LE1_Starter_Kit/SDK/_target_sdcc_nrf24le1_48'

```
If you don't have `sdcc`, install it using apt
```
$ apt install sdcc -y
```

## Download and build programmer code

Programmer consists of 2 apps, first one is Atmega328P formware, that will act as programmer hardware. Communication will happen through Serial port. Second one is Perl script that simplifies whole process to the point, where upload can be done using single command.

Get to Programmer folder and build it. For the convinience, programmer app is shaped as [Platformio](https://platformio.org/) project. 

```
$ cd ./Programmer/Programmer
$ code .

```

Build it using `build` command

```
Processing nanoatmega328 (platform: atmelavr; board: pro8MHzatmega328; framework: arduino)
------------------------------------------------------------------------------------------------
Verbose mode can be enabled via `-v, --verbose` option
CONFIGURATION: https://docs.platformio.org/page/boards/atmelavr/pro8MHzatmega328.html
PLATFORM: Atmel AVR (2.2.0) > Arduino Pro or Pro Mini ATmega328 (3.3V, 8 MHz)
HARDWARE: ATMEGA328P 8MHz, 2KB RAM, 30KB Flash
DEBUG: Current (simavr) On-board (simavr)
PACKAGES: 
 - framework-arduino-avr 5.0.0 
 - toolchain-atmelavr 1.50400.190710 (5.4.0)
LDF: Library Dependency Finder -> http://bit.ly/configure-pio-ldf
LDF Modes: Finder ~ chain, Compatibility ~ soft
Found 5 compatible libraries
Scanning dependencies...
Dependency Graph
|-- <SPI> 1.0
|-- <SoftwareSerial> 1.0
Building in release mode
Checking size .pio/build/nanoatmega328/firmware.elf
Advanced Memory Usage is available via "PlatformIO Home > Project Inspect"
RAM:   [=======   ]  68.5% (used 1402 bytes from 2048 bytes)
Flash: [==        ]  22.0% (used 6760 bytes from 30720 bytes)
================================= [SUCCESS] Took 0.58 seconds =================================
```

## Get yourself a programmer

You can make it on the breakboard or create a dedicated tool, like I did

In the first scenario connection would look like this

```
 Pin-Mapping:
 Arduino            24Pin         32Pin        48Pin
 D07 (RXD)          12 P0.6        10 P0.4        15 P1.1
 D08 (PROG)          5 PROG         6 PROG        10 PROG
 D09 (RESET)        13 RESET       19 RESET       30 RESET
 D10 (FCSN,TXD)     11 P0.5        15 P1.1        22 P2.0
 D11 (FMOSI)         9 P0.3        13 P0.7        19 P1.5
 D12 (FMISO)        10 P0.4        14 P1.0        20 P1.6
 D13 (FSCK)          8 P0.2        11 P0.5        16 P1.2

```

In case you want to go second way, schematic can be foung in the repo, also hw project is [open sourced](https://oshwlab.com/andrey.mal/2104-nrf24l01-dev-board_copy_copy). 

Hopefully board will be available to buy soon.

![Dev board](https://github.com/anabolyc/nRF24LE1_Starter_Kit/blob/main/Images/image001.jpg?raw=true)

When you have a tool, flash above firmware using `Upload` task in vscode.

```
Processing nanoatmega328 (platform: atmelavr; board: pro8MHzatmega328; framework: arduino)
-----------------------------------------------------------------------------------------------------------
Verbose mode can be enabled via `-v, --verbose` option
CONFIGURATION: https://docs.platformio.org/page/boards/atmelavr/pro8MHzatmega328.html
PLATFORM: Atmel AVR (2.2.0) > Arduino Pro or Pro Mini ATmega328 (3.3V, 8 MHz)
HARDWARE: ATMEGA328P 8MHz, 2KB RAM, 30KB Flash
DEBUG: Current (simavr) On-board (simavr)
PACKAGES: 
 - framework-arduino-avr 5.0.0 
 - tool-avrdude 1.60300.200527 (6.3.0) 
 - toolchain-atmelavr 1.50400.190710 (5.4.0)
LDF: Library Dependency Finder -> http://bit.ly/configure-pio-ldf
LDF Modes: Finder ~ chain, Compatibility ~ soft
Found 5 compatible libraries
Scanning dependencies...
Dependency Graph
|-- <SPI> 1.0
|-- <SoftwareSerial> 1.0
Building in release mode
Checking size .pio/build/nanoatmega328/firmware.elf
Advanced Memory Usage is available via "PlatformIO Home > Project Inspect"
RAM:   [=======   ]  68.5% (used 1402 bytes from 2048 bytes)
Flash: [==        ]  22.0% (used 6760 bytes from 30720 bytes)
Configuring upload protocol...
AVAILABLE: usbasp
CURRENT: upload_protocol = usbasp
Looking for upload port...
Uploading .pio/build/nanoatmega328/firmware.hex

avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e950f (probably m328p)
avrdude: erasing chip
avrdude: reading input file ".pio/build/nanoatmega328/firmware.hex"
avrdude: writing flash (6760 bytes):

Writing | ################################################## | 100% 4.84s

avrdude: 6760 bytes of flash written
avrdude: verifying flash memory against .pio/build/nanoatmega328/firmware.hex:
avrdude: load data flash data from input file .pio/build/nanoatmega328/firmware.hex:
avrdude: input file .pio/build/nanoatmega328/firmware.hex contains 6760 bytes
avrdude: reading on-chip flash data:

Reading | ################################################## | 100% 3.66s

avrdude: verifying ...
avrdude: 6760 bytes of flash verified

avrdude: safemode: Fuses OK (E:FF, H:DA, L:FF)

avrdude done.  Thank you.

======================================= [SUCCESS] Took 9.27 seconds =======================================

Terminal will be reused by tasks, press any key to close it.
```

## Build and flash sample apps

Now when we have everything we need to build and flash the board, let's try sample app. Got to `./Examples` folder and open `Serial` example in vscode

```
$ cd ./Examples/Serial
$ code .
```

Run `build` task to make sure that SDK is found, next run `upload` or `upload and monitor` task to flash sample code and optionally attach serial to it. In that case Serial data is passed by by Flasher tool.

```
> Executing task: make all <

[...]

Memory statistics:
tail -n 5 flash/main.mem
   Name             Start    End      Size     Max     
   ---------------- -------- -------- -------- --------
   PAGED EXT. RAM                         0      256   
   EXTERNAL RAM     0x0000   0x0025      38     1024   
   ROM/EPROM/FLASH  0x0000   0x08f1    2290    16384 

```

```
> Executing task: make upload <

stty -F /dev/ttyUSB0 10:0:18b1:0:3:1c:7f:15:4:0:0:0:11:13:1a:0:12:f:17:16:0:0:0:0:0:0:0:0:0:0:0:0:0:0:0:0
../../Programmer/Programmer/Programmer.pl flash/main.hex /dev/ttyUSB0
FLASH
READY
SAVING INFOPAGE...
ERASING FLASH...
RDISMB=255
NUPP=255
RESTORING INFOPAGE....
VERFIYING INFOPAGE....
OK
:03000000020006F5
WRITING...
VERIFYING...
OK
:03005F0002000399
WRITING...
VERIFYING...
OK
:0300030002006296

[...]

WRITING...
VERIFYING...
OK
:1008CE00F07401800225E0D5F0FBF4FEAFB05FF5C9
WRITING...
VERIFYING...
OK
:0608DE00B0227582002229
WRITING...
VERIFYING...
OK
:00000001FF
EOF
DONE

```

Result is below

![Leds blinking](https://github.com/anabolyc/nRF24LE1_Starter_Kit/blob/main/Images/image002.gif?raw=true)

## Links

- [Product Brief](https://docs.rs-online.com/b95e/0900766b81429bf9.pdf)
- [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF24LE1_PS_v1.6.pdf)
