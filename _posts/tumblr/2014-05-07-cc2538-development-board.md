---
layout: post
title: CC2538 Development Board
date: '2014-05-07T09:15:00+02:00'
tags: []
tumblr_url: http://jelm.be/post/85018928065/cc2538-development-board
comments: True
---
I recently designed and built a dev-board for the TI CC2538. It’s a nice little system-on-chip with an ARM Cortex M3, 2.4 GHz 802.15.4 radio and it runs at 32 MHz. Perfect for all your favourite Internet Of Things (IOT) shenanigans. Contiki supports this chip pretty well, which means easy 6LoWPAN, RPL and CoAP support.




The existing dev-boards from TI are pretty good, but have annoying connectors, which makes it hard to interface them with any real external sensors and actuators. To power them via a battery, you also need the complete evaluation board which is not really convenient.
I wanted a cheap dev-board, easily interfaceable (breadboard), easily programmable and battery powered (substantial battery) with a small form factor.




To keep everything cheap I ditched the balun by using a folded dipole antenna instead of the more standard inverted-F antenna. This makes the board a little bigger (50 mm x 40 mm), but If I wanted a breadboard friendly board I needed the size anyway for the pin header. You can find the footprint of the antenna here. It’s based of of the TI Design Note 004. I’m not that experienced in the black art of antenna design, but on this board it seems to work pretty well. Use at your own risk, and improve!


Being able to reprogram the dev-board or node easily was pretty important. The TI SmartRF06 board includes a complete JTAG programmer, which obviously is not an option for a small wireless sensor node. I was looking to a more arduino style experience, where you just connect your board, hit upload, and your new firmware starts running on the node. Luckily the CC2538 has a serial bootloader backdoor which can be used to upload firmware via a simple usb to serial adapter. Since TI’s SmartRF Flash Programmer only runs on Windows, is pretty unstable, and the Linux variant is equally bad, I wrote a cross platform python script for easy uploading and interacting with the bootloader from the command line. The script is now pulled into Contiki, so an easy “make example.upload” will automagically upload your new firmware to the board! You only need a GND, TX and RX pin to connect the board to a usb to serial adapter. More info on the github repo of CC2538-bsl. An even better solution would of course be a dedicated bootloader in the SoC that uses the USB port, but for this we need to write a new usb bootloader from scratch. Any takers?
Coin cells are great tiny batteries to power IoT devices, but your first bulky prototype is probably not going to be that power efficient from the first firmware version, and your coin cell is going to last for a day. Not very convenient for developing. That’s why I wanted a simple interface to a LiPo of your favourite size (BYOL - Bring Your Own LiPo). Add a tiny on-board lipo charger and you have a great USB rechargeable wireless sensor node. Using a LiPo means voltages up to 4.2V, which the CC2538 doesn’t like that much. The ideal solution would have been a TPS62730 step-down converter (as used in the Openmote), but I wanted to keep this board as cheap as possible, so I went for the simple MIC5225 low dropout regulator. If I get the chance I’ll do a comparison between this dev-board and the Openmote to see how much this decision impacts overall lifetime.


I didn’t include any sensors or actuators on the dev-board yet. The idea is that you add them via I2C or SPI on the breadboard, or make a “shield” that sits on top or below the dev-board. I did add a giant RGB LED, because, who doesn’t like blinky coloured lights.


The first versions I soldered up seem to work perfectly, if I get around to it I’ll maybe design a second version with some minor improvements. I have a couple extra blank PCB’s left, if you’re interested, let me know!
