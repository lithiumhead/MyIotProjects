# My Internet Of Things (IoT) Projects

This repo is a collection of source codes, schematics, documentation, photos and diagrams of the various IoT
projects that I have put together over the years. The projects are usually based on one of the three platforms:

 - ESP8266/NodeMCU programmed using Arduino
 - Node-RED on Raspberry Pi
 - OpenWrt flashed one of the off the shelf routers or a maker friendly development board

The projects are grouped under the three folders corresponding to each of the above platforms.
Some of the Node-RED projects also includes notes on Linux Kernel's [Industrial I/O](https://www.kernel.org/doc/html/latest/driver-api/iio/index.html) and [Home Assistant](https://www.home-assistant.io/) / [ESPHome](https://esphome.io/)

## ESP8266 / Arduino

## Node-RED / Raspberry Pi

	- [Waveshare Sense HAT (B) on Raspberry Pi](Node-RED/waveshare_sensehat_iio/README.md)  
	  Notes on enabling Linux Industrial I/O kernel drivers on Raspberry Pi W Zero for the various
	  I<sup>2</sup>C sensors present on the Waveshare Electronics's Sense HAT (B).


## OpenWrt

	- [Ambient Sensors / adafruit.io ](OpenWrt/linkit7688_ambient/README.md)  
	  This project uses a (MediaTek LinkIt Smart 7688 Duo)[https://labs.mediatek.com/en/platform/linkit-smart-7688] and DHT11 sensor to send temperature and humidity readings to adafruit.io. The code is an Arduino sketch that runs on the microcontroller on-board the LinkIt 7688 Duo board.
