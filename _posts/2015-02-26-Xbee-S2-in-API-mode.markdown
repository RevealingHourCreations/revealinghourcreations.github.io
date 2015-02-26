---
layout: post
title:  "Using XBee®/XBee-PRO® ZB (S2) RF Modules in API mode."
date:   2015-02-26 12:30:14
categories: rhc
author: rituparna
---

##Table of Contents
0. Introduction

1. Gathering the Requried Parts.

2. Assemble them.  

3. Fire it up.

4. Launch.


###0. Introduction
In this tutorial I will take you through the steps of getting a 3 <a href="http://ftp1.digi.com/support/documentation/90000976_V.pdf" target="_blank">XBee®/XBee-PRO® ZB Modules</a> formerly called S2 to exchange messages in API mode. We will create a mesh network using using 3 XBee modules(not S1 or 802.15.4). While many of the steps are similar between these two products, there are a few fundamental differences between the 802.15.4 and the ZB modules.

###1. Gathering the Requried Parts.
-You will need the XCTU software which can be found <a href="http://ftp1.digi.com/support/documentation/90000976_V.pdf" target="_blank">here</a>. <br>
-3 XBee radios configured in Api mode as router, coordinator and endevice respectively. It helps tracking so I duggest labeling them. The steps on how to configure them can be seen in part 2.<br>
-3 USB explorers like the one <a href="http://www.amazon.in/Bhasha-Technologies-XBU-XBEE-Xplorer/dp/B00NNQ495U?tag=googinhydr18418-21&kpid=B00NNQ495U&tag=googinkenshoo-21&ascsubtag=6f447977-3caa-7f69-5a71-000033008536" target="blank">here.</a><br>

![730326735_11773811282222442486](https://cloud.githubusercontent.com/assets/4735401/6388518/c303c7c6-bdc0-11e4-9ba4-f2b9dc1736ff.jpeg)

###2. Assemble them.
Lets start by adding a module to the XCTU software and flashing the firmware on it for the respective mode. The following images show illustrate that. 

####Coordinator
![co_firm](https://cloud.githubusercontent.com/assets/4735401/6388538/00150896-bdc1-11e4-9e4c-faf138f1f476.png)

####Router
![route_firm](https://cloud.githubusercontent.com/assets/4735401/6388647/3bc46318-bdc2-11e4-8006-935df40be12a.png)

####End-device
![end_firm](https://cloud.githubusercontent.com/assets/4735401/6388633/0d0be7b2-bdc2-11e4-8932-cf94e59ba0f7.png)
### 3. Fire it up.
Now that you have your radios labled and configured lets fire it up. Unlike in AT mode for Series 1 modules, you do not have to set the DL and DH bits in the Radio configuration. This informations is a part of the packet sent. Lets us assemble a packet using the Frames Generator tool in XCTU. Select 0x10 Transmit Request for frame type.<br>

Change the 64-bit dest. address to 00 00 00 00 00 00 FF FF <br>
Change the 16-bit dest. address to FF FE <br>
RF data seclect the ASCII tab to send strings. <br>

The frame that needs to be send is generated below.<br>
![co_frame](https://cloud.githubusercontent.com/assets/4735401/6391127/bf02f986-bdd8-11e4-88b1-55d6fc7653ec.png)
<br>
7E 00 13 10 01 00 00 00 00 00 00 FF FF FF FE 00 00 68 65 6C 6C 6F DF

Similarly we can generate a packet for the end device. Notice that the 64 bit dest address here is set to all zeros.<br>
![end_frame](https://cloud.githubusercontent.com/assets/4735401/6391574/c1841c12-bddd-11e4-91b7-bef70522c157.png)
<br>
7E 00 13 10 01 00 00 00 00 00 00 00 00 FF FE 00 00 68 65 6C 6C 6F DD

###4. Launch.

#### Co_router_end
![co_router_end](https://cloud.githubusercontent.com/assets/4735401/6391748/9d3e53e8-bddf-11e4-894a-e45fb7c42478.png)

#### Route_to_co
![route_to_co](https://cloud.githubusercontent.com/assets/4735401/6391778/d7f877c0-bddf-11e4-8152-66c894ee91a6.png)

#### End_to_co
![end_to_co](https://cloud.githubusercontent.com/assets/4735401/6391797/04ae6b76-bde0-11e4-96b7-1ac9d7dddd5a.png)







