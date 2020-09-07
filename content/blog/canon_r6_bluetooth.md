---
title: "Canon R6 Bluetooth protocol"
date: 2020-09-07T12:22:11-07:00
draft: false
---

I just got a Canon R6 recently and have been playing around with it. It's really a cool piece of equipment. One thing I noticed is that the persistent Bluetooth connection seems to allow a phone to turn the camera on even when the power switch is turned to the off position. This is useful for extracting photos from the device. I've captured a dump of the bluetooth traffic from the Canon Camera Connect app to the R6 and will probably attempt to reverse engineer it over the coming days and weeks. I've figured a few things out already that I'm fairly confident of.

## Findings so far

To initiate a connection, the phone appears to probe things like hardware ids, software versions, and things like that. Once it knows that the device is an R6, it beings pairing. It eventually makes a write request to the following characteristic:

Service UUID: 00010000000010000000d8492fffa821  
UUID: 0001000a000010000000d8492fffa821

The data appears to be a 0x01 byte followed by the name of the phone. I'm sure the 0x01 byte means something but I haven't figured out what quite yet. I suspect this is what triggers the "do you wish to connect to this device" dialog on the camera.

There's a significant amount of traffic that I haven't figured out after this, but I eventually noticed something interesting. A read to the following characteristic:

Service UUID: 00020000000010000000d8492fffa821  
UUID: 00020004000010000000d8492fffa821

The data returned by the phone seems to be an SSID that the phone can connect to with some mystery data afterwards. A read to the following characteristic returns  what could plausibly be a security key, but I haven't verified that yet, or determined whether or not it has extra data attached.

Service UUID: 00020000000010000000d8492fffa821  
UUID: 00020006000010000000d8492fffa821

## Plans

I'm going to try and write an Android app that can initiate a WiFi connection with my R6 at some point soon. I'll puzzle over the remaining pieces of traffic if it turns out they're necessary. I suspect a good amount of it comes from the fact that the Canon app needs to deal with all Canon cameras, whereas I'm mostly only interested in the R6.