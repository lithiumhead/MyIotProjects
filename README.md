# My Internet Of Things (IoT) Projects

This repo is a collection of source codes, schematics, documentation, photos and diagrams of the various IoT
projects that I have put together over the years. The projects are usually based on one of the three platforms:

 - ESP8266/NodeMCU programmed using Arduino
 - Node-RED on Raspberry Pi
 - OpenWrt flashed one of the off the shelf routers or a maker friendly development board

The projects are grouped under the three folders corresponding to each of the above platforms.
Some of the Node-RED projects also includes notes on Linux Kernel's [Industrial I/O](https://www.kernel.org/doc/html/latest/driver-api/iio/index.html) and [Home Assistant](https://www.home-assistant.io/) / [ESPHome](https://esphome.io/)

## ESP8266 / Arduino

- [Classroom Timer](ESP8266/classroom_timer/README.md)  
  This project uses a big 7 Segment LED display to show a timer that can be controlled over WiFi using a web browser on a smart phone. An Arduino Sketch turns ESP8266 into a WiFI AP and serves a webpage through which one can start/stop the timer.

- [LED Dimmer](ESP8266/led_dimmer/README.md)  
  This project uses a PWM controlled LED driver that can be configured over WiFi using a web browser on a smart phone. An Arduino Sketch turns ESP8266 into a WiFI AP and serves a webpage through which one can set the brigtnness of the LED.

- [Bulb Dimmer](ESP8266/bulb_dimmer/README.md)  
  This project uses a UART controlled Dimmer Module that can be configured over WiFi using a web browser on a smart phone. An Arduino Sketch turns ESP8266 into a WiFI AP and serves a webpage through which one can set the brightnness of an incandescent light bulb.

## Node-RED / Raspberry Pi

- [Waveshare Sense HAT (B) on Raspberry Pi](Node-RED/waveshare_sensehat_iio/README.md)  
  Notes on enabling Linux Industrial I/O kernel drivers on Raspberry Pi W Zero for the various I²C sensors present on the Waveshare Electronics's Sense HAT (B).


## OpenWrt

- [Ambient Sensors / adafruit.io](OpenWrt/linkit7688_ambient/README.md)  
  This project uses a [MediaTek LinkIt Smart 7688 Duo](https://labs.mediatek.com/en/platform/linkit-smart-7688) and DHT11 sensor to send temperature and humidity readings to adafruit.io. The code is an Arduino sketch that runs on the microcontroller on-board the LinkIt 7688 Duo board.
