# nRF24LE1 Starter Kit

Resources you need to start developing for nRF24LE1

## What is nRF24LE1

nRF24LE1 is a nRF24L01 radio module and 8051-core microcontroller in single chip. You can think of it as Arduino with nRF24L01 in a bundle. 

<image>

It has all capabilities of nRF24L01 radio chip and quite powerfull MCU that can run your code.

## Why nRF24LE1

Just for fun. Also it is dead cheap. $2-3 for ready to use module. But it is outdated, not recomended for new design, and probably you should use nRF52X or nRF53X series chips instead.

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

Programmer consists of 2 apps, first one is Atmega328P formware, that will act as programmer hardware. Communication will hapen through Serial port. Second one is Perl script that simplifies whole process to the point, where upload can be done using one command.

Get to Programmer folder and build it. For the convinience, programmer app is shaped as [Platformio](https://platformio.org/) project. 

```
$ cd ./Programmer/Programmer
$ code .

```

Build it using `build` command

<image>

## Get yourself a programmer

You can make it on the breakboard or create a dedicated tool, like I did

In the first scenario connection would look like this

```
 Pin-Mapping:
 Arduino	         24Pin		 32Pin		48Pin
 D07 (RXD)	    12 P0.6		10 P0.4		15 P1.1
 D08 (PROG)	     5 PROG		 6 PROG		10 PROG
 D09 (RESET)	  13 RESET	19 RESET	30 RESET
 D10 (FCSN,TXD)	11 P0.5		15 P1.1		22 P2.0
 D11 (FMOSI)	   9 P0.3		13 P0.7		19 P1.5
 D12 (FMISO)	  10 P0.4		14 P1.0		20 P1.6
 D13 (FSCK)	     8 P0.2		11 P0.5 	16 P1.2

```

In case you want to go second way, schematic can be foung in the repo, also hw project is [open sourced](https://oshwlab.com/andrey.mal/2104-nrf24l01-dev-board_copy_copy). 

Hopefully board will be available to buy soon.


When you have a tool, flash above firmware using `Upload` task in vscode.

<image>

## Build and flash sample apps

Now when we have everything we need to build and flash the board, let's try sample app. Got to `./Examples` folder and open `Serial` example in vscode

```
$ cd ./Examples/Serial
$ code .
```

Run `build` task to make sure that SDK is found, next run `upload` or `upload and monitor` task to flash sample code and optionally attach serial to it. In that case Serial data is passed by by Flasher tool.

<image>

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

## Links

- [Product Brief](https://docs.rs-online.com/b95e/0900766b81429bf9.pdf)
- [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF24LE1_PS_v1.6.pdf)
