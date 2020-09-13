---
title: "Pandion VTOL Drone"
date: 2020-09-11T19:45:00-06:00
draft: false
featured_image: "/imgs/pandion/render_profile.png"
---

*A scratch-built R/C VTOL aircraft.*

I have started getting into R/C aircraft recently and have been building a small twin rotor VTOL.  The eventual plan is for it to be an aerial imaging platform that doesn't need a runway for launch and retrieval.  For now, just getting it to hover stably has been quite the challenge.

## Configuration

Initially I had planned on this being a tri-copter with even distribution and forward sweeping wings on conversion, but this had a lot of un-needed complexity and the cost of the rear engine would be high.

![Tri-copter Whiteboard Concept](/imgs/pandion/tricopter_concept.png)

The main concern was to put the C of G in the right positionon during forward flight and being able to balance while hovering.  I've been getting more comfortable with Fusion 360 and it is an amazing tool to work with.  I took the time to add the approximate weight of materials and the balance and weight was quite close in the finished product.

The latest configuration is pretty close to a V-22 Osprey so the genus Pandion seemed like an appropriate name.  The ducted fan in the rear is purely for pitch control so I would consider it a bi-copter.

![Latest rendering of Pandion VTOL](/imgs/pandion/render_profile.png)

I've seen other bi-copters that will balance 2 axes, but it has been nice to have one control motion per axis with the ducted fan handling pitch.  Initially, I used small geared motors with encoders, but I had difficulty getting them to run consistently.

![Early wing design using geared motors](/imgs/pandion/early_rotary_encoder.png)

9g servos in the nacelles provide yaw control when hovering and role control in forward flight.  

## Materials

You'll notice I originally used a carbon fibre main-spar and the axle for the nacelle.  The extruded tube I used originally tended to crack and I had much better results with a proper weaved fibre tube later on.  In early versions, I also used aluminum tig wire in combination as stringers in the wing.  Later designs used aluminum c-channel for the main spar and a wooden rear spar which provide plenty of support.

![Channel wing construction](/imgs/pandion/channel_wing.png)

9g servos controlled the nacelle pitch and fine engine pitch control.  While testing, I stripped a few nylon gear servos since the nacelles are quite front heavy.  After replacing them with metal-gear servos, there hasn't been any problems.  PLA has worked well for the wing ribs and various pieces.  At some point, I may try out foam filament.

## Electronics

I've used an MPU-6050 gyro, a FlySky receiver, 3 small ESCs, 3 buck-converters, and an ESP-32 to control everything.  A flight controller would have made things easier, but part of the challenge was writing the software from scratch.  Also there is not a lot of control software that will change between configurations at run-time.

![Control Board](/imgs/pandion/control_board.png)

I then mounted the control board to the wing on a pivoting test stand to help develop the control software.

![GIF of wing on test stand](/imgs/pandion/test_stand.gif)

Foam core with hot glue has been great to use for the wing covering and fuselage.  It's easy and quick to work with when prototyping like this.

## Software

The [Pandion VTOL](github.com/ataboo/pandion) repo has all the software for the project.  Early on, I used some arduino sketches, but I've found ESP-IDF is easier to develop for a project like this.

The gyro is read over I<sup>2</sup>C so this was fairly easy to implement.  I ended up polling periodically, but I may look into using interrupts at some point.  

I managed to reverse engineer the FlySky receiver with the help of a logic analyzer.  Currently I'm providing the battery voltage as a sensor with a simple voltage divider, but the ground-work is there in-case I need more.  The channel values are read on another pin digitally.

![I-Bus sensor output on receiver](/imgs/pandion/ibus_sensor_still.png)

Getting d-shot to work for the ESCs was a little tricky, but the logic analyzer and the output from a flight controller helped me tune the timing.  I flashed the rear ESC with BL Heli to make it reversable.

I've implemented a PID controller with a positive and neutral stability mode.  I then wrote a tcp server so the ESP-32 acts as a WiFi hot-spot, for updating values remotely.  I then created a [client in fyne](https://github.com/ataboo/pandion-tcp-client) to send commands and this has made tuning the PID values much easier.

## Will it Fly?

![Basement flight test](/imgs/pandion/pandion_flight_min.gif)

The stability needs some work, but the power to weight to weight is plenty.

![Garage Test](/imgs/pandion/garage_test.png)

I will keep tuning the controls until I feel confident in its ability to hold station.  Once that is done, I should be able to tune for forward flight with the safety net of hovering.

