---
layout: post
title:  "Booting up a BeagleBone Black"
date:   2014-06-20 09:30:00
categories: beagle-bone-black
author: anuj
---

In this blog post, I'll be covering how the BeagleBone Black boots up.

![Rom Code boot procedure Beagle bone black](https://raw.githubusercontent.com/RevealingHourCreations/revealinghourcreations.github.io/master/assets/rom-code-boot-procedure-bbb.png)


**Types of boots on the BBB**

>The BeagleBone Black is very customizable. It's got 4 different methods of booting up.
    
> - eMMC boot
> - SD boot
> - Serial boot
> - USB boot


> For getting ArduPilot on the BeagleBone Black, we will be using the eMMC boot option. It's the fastest of them all, and that's important for our use case.
(There are certain other methods of loading an image into the memory of the am335x processor, but that is out of scope here) 

....

This article originally appeared on [diydrones](http://diydrones.com/profiles/blogs/booting-up-a-beaglebone-black).