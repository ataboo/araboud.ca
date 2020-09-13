---
title: "LaseCap Bird Abatement"
summary: "A device used to deter birds with a laser."
date: 2020-09-11T00:00:00-06:00
draft: false
featured_image: "/imgs/lasecap/lasecap_render.png"
---

A friend of mine who owns a berry farm mentioned that a lot of yield is lost to birds accross the whole industry.  Farmers will use various devices like nets and propane noise-makers to try and keep them away from the crops.  One of the modern devices is a laser that scans the ground to scare-off flocks.  These devices are quite large and expensive which lead me to try developing a smaller scale version with better software options.

Earlier designs incorporated a boresight camera to view where the laser was aiming.  This was simplified to a "sprinkler" style system that would scan a specified area at interval.

![LaseCap frame with exposed belt drive.](/imgs/lasecap/lasecap_frame.png)

A stepper motor drives the pan rotation via a timing belt.  All connections to the top go through a slip ring so the laser can rotate continuously.  A hall sensor is used to "zero" the bearing of the laser.  The pitch of the laser is controlled by a servo.

![LaseCap case with top removed](/imgs/lasecap/lasecap_case_open.png)

Everything is encased in sealed plastic container, with a length of clear PVC similar to a lighthouse.  

![LaseCap custom hat PCB](/imgs/lasecap/lasecap_board.png)

The device is controlled by a Raspberry Pi 3+ with a custom hat PCB and PWM controller.

![LaseCap full render](/imgs/lasecap/lasecap_render.png)