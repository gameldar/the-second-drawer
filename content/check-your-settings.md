
+++
title = "Check your settings"
date = 2025-09-15
+++

I finally performed an upgrade for my 3d printer (a Creality Ender 3) that I've been meaning to do for a while.
I switched to using [klipper](https://www.klipper3d.org) instead of the Marlin firmware, making use of an old Raspberry Pi 2B+.
I got sick of having to put things on an sdcard and I'd been having some bed leveling issues.
A long time ago I got a [Geeetech 3d Touch](https://wiki.geeetech.com/index.php/3DTouch_Auto_Leveling_Sensor) thinking it was a BLTouch and never got it working.
When I got the [Creality Sprite Extruder Pro](https://www.creality.com/products/sprite-extruder-pro-kit) I realised the wiring issues I had with the 3d touch but having sorted the other issues I wasn't brave enough to do the work to get it working as I finally had a good setup.

Installing the Klipper firmware I initially had problems connecting.
Without a working display there was no clear indication if the firmware had flashed correctly.
I ended reflashing Marlin firmware and after the first attempt failed I realised I had the GigaDevice GD32F303 version of the 4.2.2 mainboard which I confirmed by visual inspection of the board.
So armed with a new firmware build I successfully got Klipper up and running.
Or so I thought.

Bouyed by the success of getting the connection working I proceeded to setup the 3D Touch as well.
This was surprisingly simple, following the [official instructions for the BL-Touch](https://www.klipper3d.org/BLTouch.html) (despite it being a clone), once I remembered where to I could plug the device into on the extruder, and given I'd found a [configuration](https://github.com/LeeOtts/Ender3v2-Klipper-Configs/blob/main/4.2.2_printer.cfg) with the same board and extruder as I have with a `bltouch` section.

But then started the process of trying to get the bed level.
Performing the mesh leveling just gave me an indication of how badly leveled my bed was.
After quite some fruitless time trying to work out how to correctly set the z-offset and generally level the bed I discovered a couple of things:
1. You can add settings to your printer.cfg and run a manual bed leveling process.
This is outlined in the section [Adjusting bed leveling screws](https://www.klipper3d.org/Manual_Level.html#adjusting-bed-leveling-screws) in the official documentation.
Getting to a mostly level bed was the first step, then you can start to make adjustments.
2. I somehow missed this but after performing the mesh leveling you need to do a `SAVE_CONFIG` otherwise the profile will be lost.

Having done this I then spent another number of hours running through and trying to get the bed as level as possible - running tests after making changes - trying to get another Benchy out and not even getting started my leveling was so out!
Or so I thought.

Eventually, I got the bed level and only a couple of points in the `red` and a general variation under 0.25 mm, and having run the z-probe configuration again and again and tried changing nozzles as I thought I might have damaged the old one as the offsets had not be correct at times and I'd scraped over the PEI plate a number of times.
It was at this point I started to wonder if my extrusion and flow rates were the same.
Klipper and Marlin call things different in this area - `E steps` vs `Rotational distance` but doing some reading and looking at the config I had the maths worked out there.
However, I thought I would try the extrusion tests to work out what this value is.
This is where it finally clicked - the gear on my extruder was not turning.
Actually that was not true - it turned, just once sometimes.

I started to think there was something wrong with the example configuration I was following.
And it turns out this was true but not in the way I was thinking.
Somehow I had inverted the pins between the step and the direction for the extruder. 
This made sense of what I was seeing, that if I chose to extrude it would click once but no more.
If I then chose to reverse the direction of the extrusion it would click once more - as it change the direction of the extrusion.

So the lesson learned is to actually [read the instructions](https://www.klipper3d.org/Config_checks.html#verify-extruder-motor) and follow them even if you just think it is working!




