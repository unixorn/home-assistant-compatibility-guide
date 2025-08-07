# home-assistant-compatibility-guide

[![Build Status](https://img.shields.io/endpoint.svg?url=https%3A%2F%2Factions-badge.atrox.dev%2Funixorn%2Fhome-assistant-compatibility-guide%2Fbadge%3Fref%3Dmain&style=flat)](https://actions-badge.atrox.dev/unixorn/home-assistant-compatibility-guide/goto?ref=main)
[![GitHub last commit (branch)](https://img.shields.io/github/last-commit/unixorn/home-assistant-compatibility-guide/main.svg)](https://github.com/unixorn/home-assistant-compatibility-guide)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [Introduction](#introduction)
  - [Things to think about before choosing between WiFi, Zigbee and Z-Wave](#things-to-think-about-before-choosing-between-wifi-zigbee-and-z-wave)
  - [A note on dimmers](#a-note-on-dimmers)
  - [Please list stuff that _doesn't_ work, too](#please-list-stuff-that-_doesnt_-work-too)
- [Ethernet devices](#ethernet-devices)
- [Hubs](#hubs)
- [Wifi devices](#wifi-devices)
- [Zigbee](#zigbee)
- [Z-Wave](#z-wave)
- [Tools & Utilities](#tools--utilities)
- [Non-working / Poorly-working devices](#non-working--poorly-working-devices)
- [Useful links](#useful-links)
  - [Hardware Vendors with Open Firmwares](#hardware-vendors-with-open-firmwares)
  - [How-tos & Tutorials](#how-tos--tutorials)
- [Thanks](#thanks)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Introduction

This is a list of devices and software that work with [Home Assistant](https://www.home-assistant.io/) (HA) with minimal aggravation. This is _not_ a replacement for the official [Works With Home Assistant](https://works-with.home-assistant.io/) site. This list showcases devices that contributors have used with minimal hassle, not merely ones that work - most of us have encountered devices that you can coax into working.

- If you have to reflash a device to use it with HA, please add that to the **Notes** column.
- If you need to add a plugin to Home Assistant before it can be used, add that to **Notes** too.
- If it requires the devices connect to an internet server, even just for initial configuration, **please add that to the Notes for the device**. I want it easy to see which devices won't brick if the vendor goes out of business and also aren't vulnerable to some jackass hacking the company's servers.
- If the device can be reflashed to make it independent of internet servers, please add that to that entry's notes column, preferably with a link to the instructions.
- Please also add entries for tools, tutorials and utilities. Things like zigbee2mqtt, Node Red, and other software you can run that interact with or supplement Home Assistant's functionality definitely belong in this list.

Finally, if this list is useful to you, please star it to help it appear early in search results for other people.

### Things to think about before choosing between WiFi, Zigbee and Z-Wave

Read [Zigbee and WIFI Cooexistence](https://www.metageek.com/training/resources/zigbee-wifi-coexistence/) on Metageek for more details, but the TL;DR is that Zigbee channels can overlap 2.4 GHz WIFI channels, and potentially cause odd and intermittent network issues for both.

Z-Wave works on 908 MHz in the US and 868 MHz in Europe, so it doesn't have interference issues with WIFI or Bluetooth. Because there's basically one source for Z-Wave chips, the devices tend to be more expensive. That said, they're also more rigorously tested for following the standard than Zigbee devices are.

On the other hand, Zigbee devices tend to cost less, and appear less vulnerable to supply chain issues since there are more vendors for the necessary chips.

Most WIFI IOT gear seems to only work with 2.4 GHz, so you may want to upgrade your WIFI base station and router before adding too many devices, and also to access points that allow you to set up multiple SSIDs so you can have a separate walled off WIFI network for the IOT devices.

### A note on dimmers

Lutron holds a patent for sending status back to hubs on their RadioRA2 system. Not all Z-Wave or Zigbee dimmers license this patent, so some act weird. Leviton is known to license this patent, and their dimmers work well.

### Stuff that _doesn't_ work

I also maintain the [Internet of Trash](https://github.com/unixorn/internet-of-trash) list for devices that don't work, don't work well, or were just hard to integrate with HA. It's not comprehensive, but I do recommend you check it before making any purchases.

## Ethernet devices

| Name   | Description                                      | Notes           |
| ------ | ------------------------------------------------ | --------------- |
| [ControlByWeb automation devices](https://www.controlbyweb.com) | Full line of ethernet connected components for data acquisition + relay control | These are industrial-quality complete pieces of hardware, no assembly required. <ul><li>Completely configurable through its web interface</li><li>Can interface with HA via the MODBUS integration (TCP/IP flavor)</li><li>Has a dedicated mobile app for IOS and Android</li><li>Fully documented MODBUS protocol for each device</li><li>Can operate standalone (independently of HA) monitoring/controlling any process, alert of specific conditions via email, and save logs to FTP</li></ul> |

## Hubs

| Name   | Description                                      | Notes           |
| ------ | ------------------------------------------------ | --------------- |
| [Conbee II](https://amazon.com/gp/product/B07PZ7ZHG5) | USB Zigbee gateway | Works well with [zigbee2mqtt](https://www.zigbee2mqtt.io/) |
| [GoControl CECOMINOD016164 HUSBZB-1 USB Hub](https://amazon.com/gp/product/B01GJ826F8) | USB device with both Z-Wave and Zigbee radios. ||

## Wifi devices

If items here need reflashing to work with Home Assistant, please state that in the **Notes** column.

| Type           | Brand       | Name             | Notes                                 |
| -------------- | ----------- | ---------------- | ------------------------------------- |
| Doorbell (with camera) | Amcrest | [Amcrest 410 Video Doorbell](https://amazon.com/dp/B091KMT9GB) | Nice little doorbell. It supports RTSP so you can scrape its video feed into your own DVR. Not directly supported, but if your HA instance is connected to MQTT, you can use [amcrest2mqtt](https://github.com/dchesterton/amcrest2mqtt) to scrape events from it and present them to HA - it uses the discoverability protocol so that you don't even have to create the entities, they just show up automagically in HA. I had to use their IOS app to initially configure the doorbell, but after that I was able to watch video with my homelab's [Shinobi](https://shinobi.video/) server. There's a [patch](https://github.com/dchesterton/amcrest2mqtt/pull/91) to get Human Detection working correctly with the 410. |
| Garage door opener / sensor | OpenGarage | [OpenGarage.io garage door opener/sensor](https://opengarage.io) | Combination garage door opener and sensor | This is very nice piece of hardware. <ul><li>Completely configurable through its web interface</li><li>Usable with a [Blynk](https://blynk.io) template for remote control</li><li>IFTTT support</li><li>Has a dedicated cross-platform app</li><li>A well documented [REST api](https://github.com/OpenGarage/OpenGarage-Firmware/tree/master/docs)</li><li>Can send and receive MQTT messages for ingestion by Home Assistant</li><li>The firmware source code is on Github, if you want to audit or modify it.</li><li>Also has a configurable audible alarm that can sound before opening or closing the garage door</li></ul> |
| Smart Plug | Sonoff | [Sonoff S31](https://amazon.com/gp/product/B07YDC6D4D) | 15A  WIFI smart plug that includes energy monitoring. You can fit two of these in a standard US outlet. The native firmware requires you to use an app to configure it (after creating a cloud account) and was a huge pain in the ass on IOS - I never managed to get my iPhone to detect the plug. Fortunately it can easily be reflashed with [Tasmota](https://tasmota.github.io) or [ESPHome](https://esphome.io), which both work with HA and don't require a cloud account or phone app to set up. |

## Zigbee

| Type           | Brand       | Name             | Notes                                 |
| -------------- | ----------- | ---------------- | ------------------------------------- |
| Dimmer Switch | Tuya | [Rotary Dimmer (Model TS004F)](https://www.aliexpress.us/item/3256802832125833.html) | Rotary dimmer button, recognized by ZHA. "Works" but kinda annoying because dimming up/down is a frustrating experience |
| Energy Monitor | Frient | [Frient Electricity Meter Interface](https://amazon.co.uk/gp/product/B08WXT6HHJ/) | HA records energy usage over time. Recognised by zigbee2mqtt, connects to led on electricity meter. |
| Heat Alarm | Frient | [Frient Intelligent Heat Alarm](https://amazon.co.uk/gp/product/B08WXT12BN/) | Recognised by zigbee2mqtt, also measures Temperature |
| Remote Control (Button) | Konke | [Smart Button (Model 3AFE280100510001)](https://www.aliexpress.us/item/2255801129361942.html) | Smart button, recognized by ZHA |
| Remote Control (Button) | Third Reality | [Smart Button (Model 3RSB22BZ)](https://amazon.com/dp/B0BJDVKRZL) | Button, works with zigbee2mqtt. Exposes `single` click, `release` and `hold` states to HA for use as triggers. Came with a couple of strips of magnetic tape to make it easier to mount somewhere without getting lost. |
| Remote Control | Ikea | [TRADFRI remote control](https://manuals.plus/ikea/004-431-30-tr%C3%A5dfri-remote-control-manual#axzz7kd0rP732) | Five button remote control. Discontinued. Ikea product number 004.431.30.  Recognized by ZHA. Does not report battery percentage. |
| Sensor (Air quality) | Frient | [Frient Air Quality Sensor](https://amazon.co.uk/gp/product/B08WXT6166/) | Measures VOC, Humidity and Temperature, recognised by zigbee2mqtt |
| Sensor (Door/Window) | Sonoff | [SONOFF SNZB-04 ZigBee Wireless Door Window Sensor](https://amazon.co.uk/gp/product/B08BCHCZP2/) | Door/Window sensor, recognised by zigbee2mqtt |
| Sensor (Door/Window) | Tuya | [Earkong Door/Window sensor (Model TS0203)](https://www.aliexpress.us/item/3256802953712505.html) | Door/Window Sensor recognized by ZHA. |
| Sensor (Motion) | Samsung | [SmartThings Motion Sensor](https://amazon.com/gp/product/B01IE35PCC) | Detects temperature and motion. |
| Sensor (Motion) | Sonoff | [SONOFF SNZB-03 ZigBee Motion Sensor](https://amazon.co.uk/gp/product/B08XB3YPRS/) | Detects motion, recognised by zigbee2mqtt |
| Sensor (Motion) | Third Reality | [Third Reality Motion Sensor (Model 3RMS16BZ)](https://amazon.com/dp/B08RRRWK6B) | Recognized by zigbee2mqtt. |
| Sensor (Temperature) | Sonoff | [SONOFF SNZB-02 ZigBee Temperature Humidity Sensor](https://amazon.co.uk/gp/product/B08BFW697F/) | Measures Temperature & Humidity, recognised by zigbee2mqtt |
| Sensor (Water) | Samsung | [SmartThings Water Sensor](https://amazon.com/gp/product/B07F951JDP) | A cheap small water sensor. Reports wet/dry status for the zone. |
| Smart Bulb (UK) | Raveza | [Zigbee Smart Bulb B22 Bayonet](https://amazon.co.uk/gp/product/B09QSPW7DD/) | RGB Bulb, B22 Bayonet, 4.5W UK |
| Smart Bulb (UK) | Xuelili | [Zigbee Smart Bulb E27](https://amazon.co.uk/gp/product/B09NSH7JR4/) | RGB Bulb with E27 screw, 10W 220V UK |
| Smart Bulb | Innr | [Smart Bulb (Model AE 260)](https://amazon.com/gp/product/B07SC4CJ7H/) | Warm bulb, recognized by ZHA.  Nice "warm" color. |
| Smart Bulb | eWeLight | [Model ZB-CL01 Smart Bulb](https://amazon.com/gp/product/B08QC9P49G/) | RGB bulb, recognized by ZHA.  "Warm" color isn't really warm enough IMO.  Multi-color stuff is fine. |
| Smart Plug (UK) | Woolley | [Woolley Zigbee Smart Plug](https://amazon.co.uk/gp/product/B09W2FVLSH/) | 10A/2200W UK AC socket, recognised by zigbee2mqtt |
| Smart Plug | Securifi | [Peanut Smart Plug](https://amazon.com/gp/product/B00TC9NC82) | A small cheap smart plug with controllable power switch. Reliable cheap smart plug. Claims to also monitor the energy consumption of the plugged-in device, but monitoring doesn't work. These are just big enough that you can't put two of them on the same standard double US outlet. |
| Smart Plug | Sonoff | [S31 Lite zb](https://sonoff.tech/product/smart-plugs/s31-lite-zb/) | Smart Outlet/Plug, recognized by ZHA. Does not report energy usage. The LEDs are way too bright and cannot be adjusted AFAIK. |
| Smart Power Strip (UK) | Xenon | [Xenon Smart Power Strip](https://amazon.co.uk/gp/product/B09B3PZK5P) | 4 individually controllable AC sockets + 2 USB sockets that are controlled together. |
| Smart Switch (In wall) | GE | [45856GE Smart Switch In-Wall Lighting Control](https://amazon.com/gp/product/B019HTH2A0/) | Requires a neutral wire. |
| Smoke Alarm | Frient | [Frient Intelligent Smoke Alarm](https://amazon.co.uk/gp/product/B08WXV3G8P/r) | Recognised by zigbee2mqtt, also measures Temperature |

## Z-Wave

| Type           | Brand       | Name             | Notes                                 |
| -------------- | ----------- | ---------------- | ------------------------------------- |
| DIY Tool | Fibaro | [Smart Implant FGBS-222](https://www.amazon.com/dp/B07NDRCTJK/) | Plugin Universal DIY Tool.  Allows for connecting 6 DS18B20 sensors or 1 DHT sensor and  2 2-wire analog sensors, 2 3-wire analog sensor, 2 binary sensors. **NOTE: The internal Temp Sensor is too close to other components to be accurately used without compensating**. Also requires a not included power supply. |
| Dry Contact Relay | Zooz | [Z-Wave Plus S2 MultiRelay ZEN16](https://amazon.com/gp/product/B0846DZJD8/) | Three dry contact relays in one unit. Useful for controlling things like gas fireplaces, or motors. One relay supports 20A, the other two support up to 15A. Installs into HA very easily, and looks like three independent devices. Can be programmed to do things like turn off a relay after X number of minutes / hours as a safety. |
| Range Extender | Aeotec | [Aeotec Range Extender 6, Z-Wave Plus repeater](https://amazon.com/gp/product/B01M6CKJXC) | Range extender for Z-Wave. While it works reliably, it's probably more useful to just buy a Z-Wave smart plug than to get a dedicated range extender. |
| Remote Control (Wall Switch) | Zooz | [ZEN34 Scene Controller](https://amazon.com/dp/B08TMWLY74) | This is a neat little remote - it comes with a magnetic mounting plate so you can stick the base in a junction box and have it look like a regular decora switch, or pull it off and use it as a remote control. Support-wise, Zooz includes instructions for integrating this device to Home Assistant on their website. |
| Sensor (Door/Window) | Aotec | [Aeotec Door/Window Sensor 6](https://amazon.com/gp/product/B01E0OMQR6/) | Lower profile door/window open/close sensor. This is an Aeon Labs ZW112 sold by Aeotec. No longer available, but the smaller depth works really nicely for a clean look, if you can find it. Works well, with a multi-color LED on the back, which you can see flash in the dark when the door opens/closes. Supports some configuration. Battery is built-in and charges via USB. Test magnet location before installing. Home Assistant requires that the reporting type is changed from `Basic Set` to `Binary Report` in the device configuration. |
| Sensor (Door/Window) | Ecolink | [Door & Window Sensor](https://amazon.com/dp/B01N5HB4U5/) | Window and Door Sensor. Uses a magnet to sense if the door or window is open or closed. Pairs with Home Assistant very easily, no trickery needed. Comes with white and brown covers in the box. |
| Sensor (Motion) | GE / Jasco | [GE Enbrighten Z-Wave Plus Smart Motion Sensor](https://amazon.com/dp/B01KQDIU52) | Motion Sensor, battery or USB powered. Made by Jasco (model 34193). Portable motion sensor which can be mounted by screw or tape as well. Can be battery powered (will wait 4 minutes after motion detected to report again), or USB powered (will report immediately without waiting). It's buggy as a battery powered device (thinks it sees motion when it doesn't, and doesn't stop reporting motion sometimes), but works perfectly as a USB powered device. |
| Sensor (Motion, light, temperature and humidity) | Zooz | [Plus 4-in-1 Sensor ZSE40 V2.0](https://amazon.com/gp/product/B01AKSO80O/) | Quirky little device. It measures light on a scale of 0-100%, and not lux. Motion reports without timeout since last report. It requires a [templated `binary_sensor`](https://www.home-assistant.io/docs/z-wave/entities/#burglar-entity) to make the burglar sensor work as a motion sensor in Home Assistant. [Opsnlops](https://github.com/opsnlops) has an [example configuration](https://github.com/opsnlops/ha-config/blob/main/binary_sensors.yaml). |
| Sensor (Water) | Dome | [Dome Leak Sensor](https://amazon.com/gp/product/B01LXR0B8Q/) | Water leak sensor. Reports via Z-Wave when water is detected and also has an audible alarm. Includes a four foot sensor probe so you can use it in a sump or other awkward location. |
| Smart Plug (Outdoor rated) | GE | [Enbrighten Z-Wave Plus Smart Plug](https://amazon.com/gp/product/B06W9NWFM3) | Outdoor rated, also works as a range extender. |
| Smart Plug | Aeotec | [Aeotec Smart Outlet](https://amazon.com/gp/product/B07PJNL5DB/) | 15 Amp. Monitors electricity usage as well as controlling a device. |
| Smart Plug | Minoston | [Minoston Z-Wave Mini Smart Plug](https://www.amazon.com/gp/product/B08LN2NPZ3/) | Indoor only. 15 Amp. plug but only rated at 10 Amp. Does not report power usage.
| Smart Switch (Dimmer & Remote) | Leviton | [DD00R-DLZ 120VAC 60 Hz Decora Digital/Decora Smart Matching Dimmer Remote](https://amazon.com/gp/product/B01AFU1KOY) | Remote in-wall switch for the [DZ6HD-1BZ Dimmer](https://amazon.com/gp/product/B01N4F487U). |
| Smart Switch (Dimmer) | Inovelli | [Inovelli Red Series Dimmer](https://amazon.com/gp/product/B07S1BMMGH) | Wall Switch Dimmer. 3 Way compatible. No Neutral required, but recommended. Depending on your Z-wave integration, may need special setup (https://support.inovelli.com/portal/en/kb/inovelli/switches) Energy Monitoring, Scene Control, RGB Notifications|
| Smart Switch (Dimmer) | Leviton | [DZ6HD-1BZ Dimmer](https://amazon.com/gp/product/B01N4F487U) | Dimmer - 600 Watt incandescent or 300W LED or CFL. Requires a neutral wire. Periodically (roughly every three to six months) I've run into issues where it locks up with the lights stuck on, but if you trigger the airgap functionality (pull the dimmer lever gently till it clicks and the light on the switch goes out, wait five seconds and push it back into place) it reboots and starts working again. |
| Smart Switch (Dimmer) | Minoston | [Outdoor Dimmer Plug](https://amazon.com/gp/product/B09B22WVJ9/) | Dimmer - 400 Watt incandescent or 150W LED or CFL.  Z-Wave Plus, Functions as repeater. Has button on unit for on/off/adjust brightness and setup. |
| Smart Switch (Fan + Light) | Inovelli | [Inovelli Red Series Fan + Light Switch](https://inovelli.com/products/red-series-fan-light-switch-z-wave/) | Wall Switch. Allows control of a light and a fan from one switch. This device is in two parts. One goes in the wall as a normal switch, and one goes into the fan itself. The wall unit talks to the in-fan unit over RF. The fan has low, medium, and high settings. It also has a programming "Breezy" setting where it randomly jumps between fan speeds to simulate wind. Read the install directions closely before adding it to Home Assistant. You might need to update your open-wave device config with the latest version of the XML files, which Inovelli supplies themselves. |
| Smart Switch with Motion Sensor | GE / Jasco | [GE Enbrighten Z-Wave Plus Smart Motion Light Switch](https://amazon.com/gp/product/B07226MG2T/) | Decora light switch with built-in motion sensor. Made by Jasco (model 26931). Works very well as a motion sensing light switch. Motion and light switch are reported, but it's built-in light sensing is not. Configurable through the switch or Z-Wave to stay on when turned on, turn off automatically when motion isn't detected, or to turn on when motion is detected and turn off with a timer. This one seems more configurable than other motion switches. Motion is detected at least 25' away. |
| Smart Switch | GE / Jasco | [Decora Smart Switch](https://amazon.com/gp/product/B01M1AHC3R/) | Wall Switch. 3 Way compatible. Requires an add-on remote switch to work with 3 way. Supports up to four add-on switches. |
| Smart Switch | GE / Jasco | [Standard Smart Switch](https://amazon.com/gp/product/B07X6JW72G/) | Wall Switch. 3 Way compatible. Requires an add-on remote switch to work with 3 way. Supports up to four add-on switches. |
| Smart Switch | GE | [GE Enbrighten Plug in Z-Wave Smart Switch](https://amazon.com/gp/product/B004AMB3CI/) | Plug-in Single Outlet with button. |
| Smart Switch | Inovelli | [Inovelli Red Series On/Off Switch](https://inovelli.com/products/red-series-on-off-switch) | Wall Switch. 3 Way compatible. Requires a neutral. Monitors energy usage, and has a cool LED you can use for notifications. The local relay can be disabled (power is always supplied to the device), making this a great switch to use with smart bulbs. (The buttons can be used a scene controllers in this configuration.) |
| Smoke Detector/Carbon Monoxide Alart | First Alert | [First Alert Z-Wave Smoke Detector & Carbon Monoxide Alarm (2nd Generation)](https://amazon.com/gp/product/B08FFB233Y/) | Smoke and Carbon Monoxide Alarm. There's two versions of this, look for the second generation. It has Z-Wave+. The older version is Z-Wave only. Installs in Home Assistant out of the box. Responds very fast to alerts. Runs on two AA batteries. |
| Thermostat | Honeywell | [T6 Pro Series Thermostat](https://amazon.com/gp/product/B07HFL7R44/) | Mid-range thermostat that works with or without Z-Wave. Without Z-Wave, you're given full control of the schedule. Lots of options are available through the touch screen as well. When it's connected to a Z-Wave network you can configure it to still allow some scheduling on the thermostat, or to allow full control via Z-Wave. Provides temperature and humidity, and allows a large amount of configuration through Z-Wave as well. |


## Tools & Utilities

| Name   | Description                                                                                      |
| ------ | ------------------------------------------------------------------------------------------------ |
| [Amcrest2MQTT](https://github.com/dchesterton/amcrest2mqtt) | Exposes events generated by an Amcrest device to MQTT so that it will be recognized by Home Assistant's MQTT integration. |
| [Home Assistant Postgresql Backup](https://github.com/unixorn/hass-postgresql-backup)| A simple tool for backing up your Home Assistant database if you're using Postgresql. |
| [Inovelli Notification Calculator](https://nathanfiscus.github.io/inovelli-notification-calc/)| Helps you experiment with LDE effects on Inovelli switches. |
| [Zigbee2mqtt](https://www.zigbee2mqtt.io/) | Acts as a gateway between Zigbee devices and a MQTT server. Supports a bunch of different adapters and devices. |
| [Zwave-js-ui](https://github.com/zwave-js/zwave-js-ui) (was zwavejs2mqtt) | Full featured Z-Wave Control Panel and MQTT Gateway. |

## Non-working / Poorly-working devices

This section is for things that you've tried and did not get to work with HA, or do work but just not well. Please be specific about what problems you had.

| Name   | Description                                      | Notes           |
| ------ | ------------------------------------------------ | --------------- |
| [Amysen RGBW Bulbs](https://amazon.com/gp/product/B07S2487N8/) | Fairly cheap RGBW LED bulbs | These WIFI smart bulbs work with Home Assistant with very little issue, however of the 10 or so I purchased over the span of about 3 months, only 3 remain in service. All the failed ones developed a driver whine (screech, really) within a short handful of months time. |
| [Aqara Vibration Sensor](https://amazon.com/gp/product/B07PJT939B) | Mini glass break detector | This was a pita to add to my HA.<p> Once your HA is scanning for new devices, press and hold the button on the sensor for ~5 seconds until the lights flash, then you have to press it again (but don't hold it, press and release) every second or two until HA finds it.<p>Even when I did manage to add it, it kept falling off of the Zigbee mesh, even though other devices within 10 feet of it maintain stable connections. At least it was cheap. |
| [Aqara Water Leak Sensor](https://amazon.com/gp/product/B07D39MSZS) | Water leak detector | Minuses - <p>The manual was unclear on how to put it in pairing mode - press the water droplet icon firmly until the hidden light flashes three times, then let go.<p>After two months of usage I don't recommend it - it drops out of my Zigbee mesh a lot, even though it's five feet from a Smartthings motion detector that has no problems.<p>Pluses - <p>These ship ready to go - no messing with the battery, just press the button and you can add it to your Zigbee mesh. |

## Useful links

### Hardware Vendors with Open Firmwares

Devices from these vendors work without you having to take them apart and reflash them, and because they're open source, you don't have to worry about them turning off a cloud server somewhere and bricking your devices.

- [Athom Tech](https://www.athom.tech/tasmota) - Sells devices preflashed with [Tasmota](https://tasmota.github.io/docs/) or [ESPHome](https://esphome.io/).
- [CloudFree.shop](https://cloudfree.shop/) - Sells devices that are pre-flashed with [Tasmota](https://tasmota.github.io/docs/).

### How-tos & Tutorials

- [Using PagerDuty with Home Assistant](https://unixorn.github.io/post/use-pagerduty-with-home-assistant/) - Easily set up Home Assistant so you can send alerts with [PagerDuty](https://www.pagerduty.com/).

## Thanks

Many thanks to all the contributors. The list wouldn't be nearly as complete without all your help.

<a href="https://github.com/unixorn/home-assistant-compatibility-guide/graphs/contributors">
  <img src="https://contributors-img.web.app/image?repo=unixorn/home-assistant-compatibility-guide" />
</a>

Made with [contributors-img](https://contributors-img.web.app).
