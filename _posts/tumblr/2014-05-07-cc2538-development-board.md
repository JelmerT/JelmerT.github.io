---
layout: post
title: CC2538 Development Board (Updated)
tags: []
tumblr_url: http://jelm.be/post/85018928065/cc2538-development-board
comments: True
---
I recently designed and built a dev-board for the [TI CC2538](http://www.ti.com/product/cc2538). It's a nice little system-on-chip with an ARM Cortex M3, 2.4 GHz 802.15.4 radio and it runs at 32 MHz. Perfect for all your favorite Internet Of Things (IOT) shenanigans. [Contiki](http://contiki-os.org/) supports this chip pretty well, which means easy [6LoWPAN](http://en.wikipedia.org/wiki/6LoWPAN), [RPL](https://datatracker.ietf.org/doc/rfc6550/) and [CoAP](http://en.wikipedia.org/wiki/Constrained_Application_Protocol) support.

![CC2538 Dev Board](/images/2014-05-07-cc2538-development-board-1.jpg)
{: style="text-align: center;"}

<!--more-->

The [existing dev-boards](http://www.ti.com/tool/cc2538dk) from TI are pretty good, but have annoying connectors, which makes it hard to interface them with any real external sensors and actuators. To power them via a battery, you also need the complete evaluation board which is not really convenient.

I wanted a cheap dev-board, easily interfaceable (breadboard), easily programmable and battery powered (substantial battery) with a small form factor.

![CC2538 Dev Board](/images/2014-05-07-cc2538-development-board-2.jpg)
{: style="text-align: center;"}

To keep everything cheap I ditched the balun by using a folded dipole antenna instead of the more standard inverted-F antenna. This makes the board a little bigger (50 mm x 40 mm), but If I wanted a breadboard friendly board I needed the size anyway for the pin header. You can find the footprint of the antenna [here](https://github.com/JelmerT/Eagle-2.4gHz-folded-dipole-pcb-antenna). It's based of of the [TI Design Note 004](http://www.ti.com/general/docs/lit/getliterature.tsp?baseLiteratureNumber=swra118). I'm not that experienced in the black art of antenna design, but on this board it seems to work pretty well. Use at your own risk, and improve!

![CC2538 Dev Board](/images/2014-05-07-cc2538-development-board-3.jpg)
{: style="text-align: center;"}

Being able to reprogram the dev-board or node easily was pretty important. The TI SmartRF06 board includes a complete JTAG programmer, which obviously is not an option for a small wireless sensor node. I was looking to a more arduino style experience, where you just connect your board, hit upload, and your new firmware starts running on the node. Luckily the CC2538 has a serial bootloader backdoor which can be used to upload firmware via a simple usb to serial adapter. Since TI’s SmartRF Flash Programmer only runs on Windows, is pretty unstable, and the Linux variant is equally bad, I wrote a cross platform python script for easy uploading and interacting with the bootloader from the command line. The script is now pulled into Contiki, so an easy `make example.upload` will automagically upload your new firmware to the board! You only need a GND, TX and RX pin to connect the board to a usb to serial adapter. More info on the github repo of [CC2538-bsl](https://github.com/JelmerT/cc2538-bsl). An even better solution would of course be a dedicated bootloader in the SoC that uses the USB port, but for this we need to write a new usb bootloader from scratch. Any takers?

Coin cells are great tiny batteries to power IoT devices, but your first bulky prototype is probably not going to be that power efficient from the first firmware version, and your coin cell is going to last for a day. Not very convenient for developing. That's why I wanted a simple interface to a [LiPo](http://en.wikipedia.org/wiki/Lithium_polymer_battery) of your favorite size (BYOL - Bring Your Own LiPo). Add a tiny on-board [lipo charger](http://www.microchip.com/wwwproducts/Devices.aspx?dDocName=en024903) and you have a great USB rechargeable wireless sensor node. Using a LiPo means voltages up to 4.2V, which the CC2538 doesn't like that much. The ideal solution would have been a [TPS62730](http://www.ti.com/product/tps62730) step-down converter (as used in the [Openmote](http://www.openmote.com/openmote-cc2538/)), but I wanted to keep this board as cheap as possible, so I went for the simple [MIC5225](http://www.micrel.com/_PDF/mic5225.pdf) low dropout regulator. If I get the chance I'll do a comparison between this dev-board and the Openmote to see how much this decision impacts overall lifetime.

![CC2538 Dev Board](/images/2014-05-07-cc2538-development-board-4.jpg)
{: style="text-align: center;"}

I didn't include any sensors or actuators on the dev-board yet. The idea is that you add them via I2C or SPI on the breadboard, or make a "shield" that sits on top or below the dev-board. I did add a giant RGB LED, because, who doesn't like blinky colored lights.

![CC2538 Dev Board](/images/2014-05-07-cc2538-development-board-5.jpg)
{: style="text-align: center;"}

![CC2538 Dev Board](/images/2014-05-07-cc2538-development-board-6.jpg)
{: style="text-align: center;"}

The first versions I soldered up seem to work perfectly, if I get around to it I'll maybe design a second version with some minor improvements. I have a couple extra blank PCB’s left, if you're interested, let me know!

> **Update 29 September 2015**: The design files for this development board are now available! Have a look at [this follow up post](/2015/09/29/CC2538-Dev-Board-Design-Files/).
