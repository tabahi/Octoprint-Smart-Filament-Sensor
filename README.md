# Octoprint-Filament-Motion-Sensor




An [OctoPrint](http://octoprint.org/) plugin for filament motion sensor connected directly to RaspberryPi's GPIO pin.


 This new version supports the latest Raspbian OS and RPi5. This plugin revision is still pretty new and can have issues with untested environments. Try using [other forks](https://github.com/hviet17/Octoprint-Smart-Filament-Sensor) if this one doesn't work especially on RPi3 and older. Initial work based on the [Octoprint-Filament-Reloaded](https://github.com/kontakt/Octoprint-Filament-Reloaded) plugin by kontakt,  [Octoprint-Filament-Revolutions](https://github.com/RomRider/Octoprint-Filament-Revolutions) plugin by RomRider, and is a fork of  [Octoprint-Smart-Filament-Sensor](https://github.com/Royrdan/Octoprint-Smart-Filament-Sensor) by Royrdan.


Main updates - August 2024:
- Removed RPi.GPIO requirements, replaced it with libgpiod.
- Only one mode that uses both the distance and the timeout together.

## Required sensor

To use this plugin a Filament Sensor needs to be sending a toggling digital signal (0-1-0...) during filament movement.
- A ready-made sensor can be used such as BIGTREETECH Smart Filament Sensor.
- You can also make one yourself using a slotted wheel. Put an an LED on one side and a photodiode on the other side. Connect the output of the photodiode to a GPIO pin of RPi. Make sure to not input more than 3.3V to raspberry pi. The light from the LED (or IR LED) will trigger the photodiode (or IR receiver LED) through the slots when the wheel turns. Here is an example model by [WindoAC on Thingiverse](https://www.thingiverse.com/thing:3067904).


![BTT smart sensor wiring](./BTT_smart_sensor.jpg)

![Slotted wheel, IR diode and IR sensor prototype](slotted_wheel_IR_diode_animation.gif)


## Features
*  Detects filament jams, runrouts, clogs, spool tangles and pauses the print automatically.
* Uses highly responsive GPIO interrupts.
* Can respond within a second of no-filament-motion.
*  Configuration limits and timeouts for pausing.
* Custom GCode on pause.
* Heaters timeout after pause.

## Installation

* Install via the bundled [Plugin Manager](https://github.com/foosel/OctoPrint/wiki/Plugin:-Plugin-Manager).
* Manual install: Scroll down the "Get more" menu in Plugin Manager, Use this URL: https://github.com/tabahi/Octoprint-Smart-Filament-Sensor/archive/master.zip


After installation a restart of Octoprint is required.


### Installation Requirements
- Python>3.8
- [gpiod>=2.0.2](https://pypi.org/project/gpiod/) installed in Octoprint environment.

It has been tested with RPi5 running [octoprint_deploy 1.0.11](https://github.com/paukstelis/octoprint_deploy) and Zero2w running Octopi1.0 image. You are welcome to raise issues for untested environments.


## Configuration
### GPIO Pin
* Choose any free GPIO pin you for data usage, but I would recommend to use GPIO pins without special functionalities like e.g. I2C
* Run the sensor only on 3.3V, because GPIO pins don't like 5V for a long time

### Pausing Limits
A print can be paused due to two limits when the filament is not moving:

1. Extrusion distance limit after jam. It's best to set between 5mm (slow) and 20mm (fast) depending on the print speed. With retractions, the filament might not move much triggering this limit therefore a grace-period can be set to override this limit for a couple of seconds.
2. Timeout after jam. Seconds to wait after no-filament-motion before pausing. 5s for very fast print, 120s for very slow prints.

### Octoprint - Firmware & Protocol
If you are not printing by SD card, then the octoprint @pause command is pretty simple and works for most. However, there is a custom GCode option with or without Octoprint's pause if you want to send commands such as M600 for filament change. By default, the custom GCode is just a beep tune.

### Octoprint - GCode Scripts
The custom GCode managed by the plugin is only run when a print is paused due to a suspected jam. You can also add a GCode Script for "After print job is paused" in OctoPrint Settings > GCode Scripts that will run for all pauses. Make sure to not overlap the two. Also adding a GCode script for "Before print job is resumed" might be useful, in the case you hit the heatbed or print head during the change of the filament or removing the blockage.

## Disclaimer
* I and all the previous authors of this plugin are not responsible for any damages on your print or printer. As user you are responsible for a sensible usage of this plugin.
* Be cautious not to damage your Raspberry Pi, because of wrong voltage. I don't take any responsibility for wrong wiring.
* Try custom GCode commands at your own risk.


## Outlook
* Telegram notifications.
* A simplified DIY sensor
* Support of multiple sensors for multiextruders. Depending on the demand.

## Contact
* [Issues](https://github.com/tabahi/Octoprint-Smart-Filament-Sensor/issues)
* [Octoprint Discord](https://discord.octoprint.org/)