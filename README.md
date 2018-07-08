# Raspberry PI LIRC HAT #
## Now, you can control everything with an infrared remote with your Raspberry Pi! ##

This an extension PCB for Raspberry PIs with 40 Pins.

Its purpose is to provide a infrared interface compatible with [LIRC](http://www.lirc.org/).

I did my very best to fulfill the [Raspberry Pi HAT specification](https://github.com/raspberrypi/hats).

![3D render](/RaspiIR/RaspiIR.png)

To build it, you need:
* bravery, soldering skills, linux
* The PCB like in the design file in RaspiIR/RaspiIR.kicad_pcb
* 2x 100nF Ceramic Cap 0603 SMD
* 2x 100uF Electrolytic Cap SMD 6.3mm diameter
* 2x IR-LED: Everlight IR333-A
* 2x IR-LED: Everlight IR333C/H0/L10
* 1x TSOP383xx or AX-1838 IR receiver
* 5x MMBT3904TPMSCT-ND NPN transistor SOT-23
* 1x 24C32 I2C eeprom SOIC-8 or SO-8
* 4x 33R 1W 2512 SMD resistor
* 1x 330R 0603 SMD resistor
* 2x 1k 0603 SMD resistor
* 2x 3.9k 0603 SMD resistor
* 1x Pin-Header 0.1" pitch 2x20 pins (at least 8mm high)

After soldering it all together, you have to generate and write content on the ID eeprom. Its purpose is to load the driver when your Raspberry Pi boots.

To achieve this, follow these steps:

1. compile the tools in pi-lirc-hat/eepromutils with ```make```
2. use ```./eepmake eeprom_settings.txt lircHAT.eep lirc-rpi-overlay.dtbo -c setup-readme-blob``` to compile the eeprom content
3. connect the eeprom to the I2C-Pins of a Raspberry (NOT the ID-SCL, ID-SDA pins)
4. read on how to use eepflash with ```./eepflash.sh -h```
5. use eepflash to write the content to the ID eeprom, e.g.:```./eepflash.sh -w -f=lircHAT.eep -t=24c32 -d=1 -a=50```

After that, you have to install lirc on the Raspberry. This is documented in pi-lirc-hat/eepromutils/setup-readme-blob.

You can also find this in /proc/device-tree/hat/custom_0, because it is included in the eeprom! :D

The design with the 4 IR leds is inspired by [TV-B-Gone](https://learn.adafruit.com/tv-b-gone-kit/overview). The driver overlay is from the [Raspberry Pi Kernel source tree](https://github.com/raspberrypi/linux) with slight modifications. The PCB was designed using [KiCAD](http://kicad-pcb.org/). I used a template for the PCB design which can be found [here](https://github.com/xesscorp/RPi_Hat_Template)
