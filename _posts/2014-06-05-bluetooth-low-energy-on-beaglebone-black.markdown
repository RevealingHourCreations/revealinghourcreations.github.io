---
layout: post
title:  "Bluetooth Low Energy on BeagleBone Black"
date:   2014-06-05 17:15:00
categories: bluetooh-low-energy
author: anuj
---

In this blogpost we'll take a look at Bluetooth 4.0 on Linux.  
A lot many BLE enabled devices are hitting the market. Right from smartwatches, finder tags to healthcare devices. Making these works with your smartphone is as easy as downloading an app. Making it work with your Raspberry Pi or BeagleBone Black opens up loads more possibilities.

### Hardware required
* [BeagleBone Black](http://beagleboard.org/Products/BeagleBone+Black) running the [latest Debian](http://beagleboard.org/latest-images).
* USB BLE module from
  [Adafruit](http://www.adafruit.com/products/1327). I bought the same from [MG Super Labs](https://www.mgsuperlabs.co.in/estore/Bluetooth-CSR4-0-USB-Dongle?search=bluetooth)
* A BLE ready device 

### BlueZ 
* [BlueZ](https://lwn.net/Articles/531133/) provides the official support for Bluetooth in Linux. We will be using
[version 5.0](http://www.bluez.org/) or above, since that added the support for BLE. We need a kernel above 3.5 for this to work.

    # uname -a
    Linux beaglebone 3.8.13-bone41 #1 SMP Tue Mar 4 22:51:47 UTC 2014 armv7l GNU/Linux
Make sure that the USB module is connected and has been detected

    # lsusb
    Bus 001 Device 002: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Next install bluez

    # apt-get install bluez

Verify that it has been installed with hciconfig

    # hciconfig
Next scan for Bluetooth devices

    # hcitool scan
    Scanning ...
    B8:E8:56:35:38:DF Anuj’s MacBook Pro

  This will return all Bluetooth 3.0 devices. This doesn't include any Bluetooth 4.0 devices  

For BLE devices, do

    # hcitool lescan
    LE Scan ...
    7C:66:9D:9B:2D:DA (unknown)
    7C:66:9D:9B:2D:DA DHIRAJ
    7C:66:9D:9B:2D:DA (unknown)
    7C:66:9D:9B:2D:DA DHIRAJ
    7C:66:9D:9B:2D:DA (unknown)

Now, to connect to a BLE device using [GATT](https://developer.bluetooth.org/gatt/Pages/GATT-Specification-Documents.aspx) profiles

    # gatttool -b 7C:66:9D:9B:2D:DA -I
    [   ][7C:66:9D:9B:2D:DA][LE]>

We specify the MAC address of the BLE module which we got from the *lescan* command, and pass it along with the *-b* parameter. To get an interactive shell to communicate we use the *-I* parameter.


### What you could do with this ?
* Not all smartphones have BLE support yet. Most will, by 2018. So in the meanwhile we could connect our non BLE enabled smartphones via WiFi to a BeagleBone Black who talks to the BLE device.
* You could control your [Hue bulbs](http://www.usa.philips.com/e/hue/hue.html) when your smartphone isn't around, maybe when you are not at home but need to control the lights anyways.
* It's also loads of fun.

### References

*[Experimenting with BLE under Linux](http://joost.damad.be/2013/08/experiments-with-bluetooth-low-energy.html) by Joost Yervante Damad

![Beagleboneblack](https://raw.githubusercontent.com/RevealingHourCreations/revealinghourcreations.github.io/master/assets/bbb.jpg)
