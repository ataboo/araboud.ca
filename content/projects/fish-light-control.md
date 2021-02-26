---
title: "Fish Light Control"
summary: "An esp32 controlling the brightness of an LED strip for a fish tank."
date: 2021-02-25T00:00:00-07:00
draft: false
featured_image: "/imgs/fishlightcontrol/FishCaseRender.png"
---

This was a quick project to have an esp32 control a light strip over our fish tank.  It was nice to keep the scope small and finish it off in a few days.  Our aquarium had a small LED strip light on a rocker switch and I had just picked up a length of WS2812 addressable LED, so this seemed like a good opportunity to try them out.

I debated on using a Raspberry Pi Zero and look at integrating it as a IOT device, but I ended up going pretty minimalist by making it all timer based with an esp32 dev kit.  I put together a schematic and design in Eagle CAD.  This makes for good practice and it tends to save soldering re-work when I have a good diagram to work with.

![Drawing of board from Eagle CAD](/imgs/fishlightcontrol/FishControlBoard.png)

The WS2812 strip runs on 5v and the esp32 data is 3.3v so I needed a level shifter.  The data rate is pretty quick at 800khz so a simple level shifter wouldn't do the trick.  Apparently I might have gotten away with just using 3.3v data, but I ended up getting an inverter (SN74HCT04N) instead to shift to 5v.  GPIO 17 on the esp32 goes to the first input, the inverted signal loops back to its second input, and the second output is the right polarity and voltage for the lights, saving me needing to invert it in software.

![Soldered prototype board](/imgs/fishlightcontrol/FishControlBoardSoldered.png)

It was a bit greedy to skip a full breadboard setup, but the circuit was simple enough that I could jump to a prototype board like this.  It turned out okay with the only real changes being the added pullup resistor and a different style connector for the LED strip.

Next I wrote [the software to drive the controller](https://github.com/ataboo/fish-light-control) using ESP-IDF.  Lucky for me, they had a WS2812 example for the RMT library so it was pretty straight forward to implement.  I set it up to startup the WIFI every 24 hours and update the clock and update the color according to the time every minute.  A noon color is set and it ramps up and down during the configured sunrise/sunset periods.  I was impressed how easy it was to sleep and wake in esp-idf.  I could definately see this being a good option for battery powered projects.

Next I designed a case for the board in Fusion and found a couple nice SVGs to represent the 2 end users.

![3d Printed case for the control board](/imgs/fishlightcontrol/FishControlCasePrinted.png)

I printed the case in wood PLA and everything fit pretty well.

One thing I ran into was the lights flickering colours sometimes when I would handle the board.  After trying a few things, I ended up fixing it by adding an external pull-down resistor between the GPIO and the inverter.

![The LED strip and controller](/imgs/fishlightcontrol/FishControlLightAndController.png)

Overall was a nice quick project.  I look forward to coming up with new uses for the WS2812s.

[fish-light-control on Github](https://github.com/ataboo/fish-light-control)

