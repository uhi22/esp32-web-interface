esp32-web-interface
=====================
Web interface for Huebner inverter

# Table of Contents
<details>
 <summary>Click to open TOC</summary>
<!-- MarkdownTOC autolink="true" levels="1,2,3,4,5,6" bracket="round" style="unordered" indent="    " autoanchor="false" markdown_preview="github" -->

- [About](#about)
- [Usage](#usage)
    - [Wifi network](#wifi-network)
    - [Reaching the board](#reaching-the-board)
- [Hardware](#hardware)
- [Firmware](#firmware)
- [Flashing / Upgrading](#flashing--upgrading)
    - [Wirelessly](#wirelessly)
    - [Wired](#wired)
- [Documentations](#documentations)
- [Development](#development)
    - [Arduino](#arduino)
    - [PlatformIO](#platformio)

<!-- /MarkdownTOC -->
</details>

# About
This repository hosts the source code for the Web Interface for the Huebner inverter, and derivated projects:
* [OpenInverter Sine (and FOC) firmware](https://github.com/jsphuebner/stm32-sine)
* [Vehicle Control Unit for Electric Vehicle Conversion Projects](https://github.com/damienmaguire/Stm32-vcu)
* [OpenInverter buck or boost mode charger firmware](https://github.com/jsphuebner/stm32-charger)
* [OpenInverter non-grid connected inverter](https://github.com/jsphuebner/stm32-island)
* [BMS project firmware](https://github.com/jsphuebner/bms-software)
* ...

It is written with the Arduino development environment and libraries.

# Usage
To use the web interface 2 things are needed :
* You need to have a computer on the same WiFi network as the board,
* You need to 'browse' the web interface page.

## Wifi network
There are 2 possibilities:
* Either you connect to an Access Point generated by the board. The default name for this access point is 'ESP-xxxxx' but can be customized. In that case, the board will have a fixed IP address of `192.168.4.1` (and will be reachable on http://192.168.4.1/)
* Or you can configure the board to join your own WiFi network ; and in that case you may need to tweak your network configuration to provide a fixed address to the board (not necessary).

## Reaching the board
The board announces itself to the world using mDNS protocol (aka Bonjour, or Rendezvous, or Zeroconf), so you may be able to reach the board using a local name of `inverter.local`.
So first try to reach it on http://inverter.local/

# Hardware
The web interface has been initially designed to run on ESP32-WROOM-32E boards.

A SD card running in SDIO mode can be connected (CLK to pin14, CMD to pin15, D0 to Pin2, D1 to Pin4, D2 to Pin12, D3 to Pin13).

A RTC can be connected.  As standard a PCF8523 is suported but any clock supported by RTClib can be used with a sketch change. (SCLK to Pin22, SDA to Pin21). 

The connection to the inverter are on Pin16 (Rx line connect to inverter Tx line) and Pin17 (Tx line connect to inverter Rx line).

# Firmware
Tompile it follow the [instructions below](#development).

# Flashing / Upgrading
## Wirelessly
TBA

## Wired
If your board is new and unprogrammed, or if you want to fully re-program it, you'll need to have a wired connection between your computer and the board.
You'll either need a ESP32 board with an on board USB to serial converter or a 3.3v capable USB / Serial adapter
* the following connections:  

Pin#  | ESP32 Board Function | USB / Serial adapter
----- | ---------------------- | --------------------
1     | +3.3v input            | (Some adapters provide a +3.3v output, you can use it)
2     | GND                    | GND
3     | RXD input              | TXD output
4     | TXD output             | RXD input

Then you would use any of the the [development tool below](#development) ; or the `esptool.py` tool to upload either a binary firmware file, or a binary filesystem file.

Various openinverter boards (SDU, LDU, Leaf) use a different wiring scheme for initial programming

Pin#  | ESP32 Board Function | USB / Serial adapter
----- | ---------------------- | --------------------
1     | TXD output             | RXD input
2     | RXD input              | TXD output
3     | +5v input              | (Some adapters provide a +5v output, you can use it)
4     | GND                    | GND
5     | GND                    | GND
6     | GPIO0                  | Connect this to pin 5 (GND) to put the ESP32 into programming mode. Then power up

Flash subsequent updates via OTA.

# Documentations
* [Openinverter Web Interface Protocol](PROTOCOL.md)

# Development
You can choose between the following tools:

## Arduino
[Arduino IDE](https://www.arduino.cc/en/software) is an easy-to-use desktop IDE, which provides a quick and integrated way to develop and update your board.
* [Initial setup](doc/ARDUINO_IDE_setup.md)
* [Day to day usage](doc/ARDUINO_IDE_usage.md)

## PlatformIO
[PlatformIO](https://platformio.org/) is a set of tools, among which [PlatformIO Core (CLI)](https://docs.platformio.org/en/latest/core/index.html) is a command line interface that can be used to build many kind of projects. In particular Arduino-based projects like this one.
(Note: even if PlatformIO provides an IDE, these instructions only target the CLI.)
* [Initial setup](doc/PLATFORMIO_setup.md)
* [Day to day usage](doc/PLATFORMIO_usage.md)
