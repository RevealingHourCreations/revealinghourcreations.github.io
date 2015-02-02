---
layout: post
title:  "Piano Stairs"
date:   2015-01-31 12:30:14
categories: rhc
author: rituparna
---

![piano_fullimage](https://cloud.githubusercontent.com/assets/4735401/5972623/c736bc2a-a882-11e4-8465-e713a89e5e86.jpg)

A few weeks ago we pitched the idea of doing Piano Stairs to the Phoenix Market City around Christmas — a shopping mall close to our office. Piano stairs as a concept has been implemented in various cities across the world as part of the “Fun Theory” project. Though I knew the work flow or rather the events that needed to be captured and processed, I was unaware of the most accurate way to do it. I began experimenting with using Light Dependent Resistors (LDR), which we quickly gave up on, followed by an ultrasonic pair and infrared transceivers.


We decided to go ahead with a capacitive touch sensor. I would recommend this setup to anybody else trying to do this project. In our experience this is the most accurate, consistent and reliable way of solving the problem of detecting a person stepping on a stair.

![mpr](https://cloud.githubusercontent.com/assets/4735401/5972417/12e7aae8-a87f-11e4-8cff-5dbd3c989308.jpg)


## The long version

### What are piano stairs?

![piano_stairsb4and after](https://cloud.githubusercontent.com/assets/4735401/5972509/d67066c0-a880-11e4-9e21-0501fcb98432.jpeg)

A set of stairs that are designed to look like a piano with sensors attached to them so they trigger a MIDI device (typically piano sounds) as you step on them.


### What are the various approaches we tried?
Light dependent resistors (LDR)
An LDR with a light source across the steps was used to do a proof of concept. This method was then ruled out because the ambient light conditions keep changing and thus the consistency of readings could not be achieved.

Infrared (IR) pair
IR worked much better than LDR but scaling was an issue because with so many IR emitters it was hard to detect if any beams were refracted. Also, they had to be aligned in line of sight for them to work accurately, which was not feasible for installation.

Ultrasonic pair
A pair of ultrasonic sensors had the same issue: when multiple sensors were deployed it often happened that one sensor would pick up the sound deflected by the other, thus making this solution unscalable as well.

Capacitive touch
We finally settled on a solution that worked perfectly: a [Tah](http://www.tah.io) with MPR121 capacitive touch sensor on an I2C line. We were getting enough accuracy and consistency with the MPR with Tah and a Raspberry Pi to begin polishing this into our end product.

### Python code

The data processed by Tah and MPR121 was sent to a Raspberry Pi to play MIDI notes. With this approach we ran into two major issues:
Serial processing of the data created a lag or queue which didn’t make the setup feel real-time.
MIDI output port had to be configured to output of the Pi.
We were fortunate to discover the awesome simplicity of [VMPK](http://vmpk.sourceforge.net) at this point. But VMPK can’t be used on a Pi because it is bound to an X terminal and a screen is required to be able to use it. So the final deployment was done with VMPK running on a MacBook hooked to the Tah-MPR121 combination.

###  Checkout the Video 


### Putting it all together

Now came perhaps the hardest part! On the night of deployment we assembled our entire team of 8 people at the Phoenix mall at 10.30 pm. We brought along every possible tools that might be required and teams were divided according to tasks. The night passed in no time and it was soon 6 am as everyone got busy transforming this huge 35-step staircase into a grand acoustic piano (at least partly, since we decided to skip implementing the black keys to keep things simple). We were greatly helped by two engineering undergrads that I got in touch with who were “vinyl experts”, a skill that all of us agreed to endorse on each others’ LinkedIn profiles by the end of the project!

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/vMV_lfypvD4/0.jpg)](http://www.youtube.com/watch?v=vMV_lfypvD4)


### Customer reactions
I had much fun talking to people, watching them go up and down the stairs, although an escalator was at hand. Little kids were often the first to figure it out. Everyone, right from the watchmen to doctors, college students to mothers, and children to grandparents thoroughly enjoyed the experience.

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/37QWAPxf0uI/0.jpg)](http://www.youtube.com/watch?v=37QWAPxf0uI)




