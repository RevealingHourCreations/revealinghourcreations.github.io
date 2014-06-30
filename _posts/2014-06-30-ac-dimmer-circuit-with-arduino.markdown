---
layout: post
title: "AC Dimmer circuit with Arduino"
date: 2014-06-30 11:22:19 +0530
author: ""
author: ninad
---

Working on a faulty, shorted breadboard... tripping the MCB....underestimating the power of a circuit and treating it disrespectfully... and then working with a constant paranoia at the back of your head, and the constant feeling that anything can burst any time now, are just a few of the points auto-commited to the infamous bucket list of a person who shifts from sweet 5V DC circuits, to a crazy 230V AC. In my case too, it took time to empty the bucket, but surely, it did happen.

My first interaction with AC circuits happened when I was presented with the task of constructing a dimmer circuit that would be able to drive resistive-AC loads. The first thing that generally comes to mind if you have worked with DC circuits is PWM. And yes, the solution is not much different than this, just that the its called firing angle in AC circuits.

![Alt](https://raw.githubusercontent.com/RevealingHourCreations/revealinghourcreations.github.io/master/assets/dimmer.png "firing_angle")

If we are able to do this to an input sine wave, we have our dimmer! Fan regulators use triacs to do this. Once we turn the knob, we end up altering the firing angle, which is then mapped to the speed of the motor. Using such a regulator would be one option to control dimming, but then the user would have to get all the way to the regulator to enable the action. Hence, controlling the firing angle should be done through electronic signals. This is what I did using the Arduino to provide the required electronic signals. The circuit that finally worked is given below.

![Alt](https://raw.githubusercontent.com/RevealingHourCreations/revealinghourcreations.github.io/master/assets/dimmer_numbered.png "firing_angle")

You can find the Arduino code [here](https://raw.githubusercontent.com/RevealingHourCreations/revealinghourcreations.github.io/master/assets/ac_light_dimmer.ino)

**Working Principle:**

IC DF1504S is a bridge rectifier. This full wave recitified output is fed to IC 4N25, an optical transistor. This may seem unnecessary to the operation of the circuit, but is of prime importance for the safety of the circuit. They are also called optical isolators, since the isolate the AC part of the circuit from the DC. The zero-crossing output of the circuit is an interrupt to the Arduino, to start counting the duration which is proportional to the current alpha value(firing angle) given by the user, and accordingly fire the triac at the required angle.

Q1 is the triac that is fired after MOC3021 is fired by signals from the Arduino. MOC3021 is an optically isolated triac for separating AC and DC. Finally the load is given supply from Q1, to obtain the desired action.

**Precautions:**

A common mistake that most people, used to working on 5V electronics, do, is that they end up connecting the 0.25 watt resistors instead of the 1 watt ones. The circuit heats up considerably, and the 0.25 resistors won't be able to stand it. Also don't check which components are heating up by touching them with the circuit ON. It may seem stupid now, but that is what we tend to do as a matter of habit.

If the circuit won't work the first time, and you need to debug, check for connectivity wth the circuit in OFF state. While checking in ON state, be extra careful where you touch your probes...I still don't know where I shorted them, and after that, the lights in the place went out as the MCB tripped, not before giving a big bright spark and an equally prominent noise....Yes...I was caught.

All said and done, the final result that came out of it is pleasing to the eyes...well....quite literally.:P
