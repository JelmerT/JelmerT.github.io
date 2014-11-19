---
layout: post
title: Tomorrowland bracelet teardown
tags:
- TML2014
- wearable
- IoT
- NFC
- teardown
comments: True
---
I got hold of a Tomorrowland bracelet (thanks [Jef](https://twitter.com/jefseghers)!) and was wondering what makes it tick. Watch me tear it apart and discover all the tech inside.

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-5.jpg)](/images/2014-11-17-TML-bracelet-teardown-5.jpg)

<!--more-->

[Tomorrowland](http://www.tomorrowland.com/) is the largest electronic music festival held in the world. It's held in Boom, Belgium and is known for its extensive decorations and impressive shows. [The aftermovie](http://youtu.be/NtDG-Cnj-pw) of this year's edition (2014) should give you a pretty good idea of the atmosphere at the festival.

At this year's edition the paper entrance tickets were replaced by electronic bracelets. Every guest got a [pretty box](http://i.imgur.com/AeVNlcd.jpg) in the mail containing their personal bracelet. On the inside of the bracelet a [unique ID (UID)](http://en.wikipedia.org/wiki/Unique_identifier) is lasered onto the leather strap. The guest has to register this number on the website of the event to link the bracelet to his name and ticket. At the event an RFID scanner is then used to scan the bracelet and grant the guest access to the festival site.

But the bracelet had a couple more functions. The leather band clearly has 2 RGB LEDs embedded on either side of the *watch face*. These LEDs are remote controlled by the [FOH](http://en.wikipedia.org/wiki/Front_of_House) to light up in sync and with the correct color to match the light show. This is a pretty neat effect, especially when a couple thousand people are waving their hands in the air.

<iframe width="740" height="425" src="//www.youtube.com/embed/eDZaea7OCd0?rel=0" frameborder="0" allowfullscreen></iframe>

This effect has already been used before and was invented and commercialized by [Xylobands](http://www.xylobands.com/) ([original patent](https://www.google.co.uk/patents/WO2013021209A1)) and backed by Coldplay.

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-6.jpg)](/images/2014-11-17-TML-bracelet-teardown-6.jpg)
([source](http://www.tomorrowland.com/en/2014-w2))

Another function of the bracelet was to let visitors easily exchange Facebook details when making new friends at the festival. The bracelet has a button embedded in the strap (underneath the heart). When both guests push this button at the same time, they'll get some feedback via the LEDs and at the end of the festival an email appears in their inbox with the contact details of the other person.

![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-7.jpg)

Yet another function of the bracelet, and maybe one that was less known to its users, is the ability to track people. The bracelets have a unique ID and a wireless radio embedded by which people can be identified, but also located in a certain area. This is done by setting up different antennas and then triangulating the signal coming from a bracelet or just looking at the [RSSI](http://en.wikipedia.org/wiki/Received_signal_strength_indication) at the different antennas.

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-8.jpg)](/images/2014-11-17-TML-bracelet-teardown-8.jpg)
([source](http://sendrato.com/?page_id=32))

I also heard that the bracelets could have the capability of being linked to your credit card, so it can be used as an electronic purse at the bars and food stands on the festival site.

All these different functions and capabilities got me wondering about what makes these bracelets tick (pun intended). Since the RFID function for entrance control and payment clearly needs a short range wireless radio and the LEDs and user tracking a long range radio, there must be at least 2 radios inside the bracelet. And of course 2 corresponding antennas. The festival lasted about 2 times 3 days, so battery might become a concern, especially if the LED function is being used often. And how does the exchanging of Facebook contacts work? Is there any storage on the bracelet to store IDs? All these functions seemed possible, but could become expensive (hardware-cost wise) and bulky. The ticket price for Tomorrowland isn't cheap (about €230), but I presume they still needed a cheap, mass produced piece of electronics.

Lets have a look what's inside!

<iframe src="//player.vimeo.com/video/112217629?title=0&byline=0&portrait=0" width="740" height="425" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

On the back of the bracelet we can clearly see some markings: the bracelet is a product of [**Sendrato**](http://sendrato.com/) and the model seems to be **SUM3**, which has to be custom designed for Tomorrowland (the casing anyway).

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-10.jpg)](/images/2014-11-17-TML-bracelet-teardown-10.jpg)

The PCB inside the bracelet is composed of a central hard FR4 circular board. Two pieces of flex PCB, containing antennas, LEDs and the button, are soldered on both sides to the main board. It's interesting to see that they used separate flex parts and soldered / glued them to the main board as apposed to a complete [rigid-flex solution](http://www.candorind.com/assets/img/Flex-RigidMax800px500pxl.jpg) where the flex part is just a layer of the FR4 PCB. I guess the extra labor cost was cheaper than the manufacturing price of the more complicated PCB.

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-9.jpg)](/images/2014-11-17-TML-bracelet-teardown-9.jpg)

On the main PCB we can find 3 chips, a crystal/oscillator and some resistors, caps and coils. The whole PCB seems to be potted in some kind of glue, probably to make it cope at least a little better with sweat and humidity.

*	The main processor is the Silicon Labs [SI1063-A-GM](http://www.silabs.com/Support%20Documents/TechnicalDocs/Si106x-8x.pdf), which is a pretty capable 8051 based [SoC](http://en.wikipedia.org/wiki/System_on_a_chip) with a sub-Ghz transceiver (here tuned to 915 MHz). (about $3.3)
*	The smaller UFDFPN8 package to the left with the markings '404E 8330' seems to be connected to the 13.56 MHz antenna. My money is on the [M24SR** series from STMicroelectronics](http://www.st.com/web/en/resource/technical/document/datasheet/DM00087276.pdf) ([Anthony](https://www.facebook.com/notes/anthony-soete/tomorrowland-2014-bracelet-teardown/10152586017265939) seems to agree). It's a simple EEPROM with both an I2C and an NFC interface. (about $0.4)
*	The SSOP6 package on the top right looks like a generic voltage regulator. It seems that the SI1063 is powered straight from the CR2032, while the NFC chip is powered by an IO pin of the SI1063 (while accessed via I2C, the chip itself can also be powered by and external RFID reader). The two LEDs seem to be powered by the extra voltage regulator (to step down the battery voltage from 3V).

So it seems we were right about the two separate radios (and antennas) on different frequencies. The access control is clearly implemented on the 13.56 MHz NFC chip. The chip comes with a factory pre-burned UID, which is most likely used to identify the user. This UID possibly is the one lasered on the strap.  
The LEDs are clearly controlled by the Silicon Labs chip and its 915 MHz radio. Once in a while the chip wakes up from sleep mode and scans for an incoming radio packet send from one of the transmitters installed at the different stages. This radio packet holds the color information for the LEDs. Once a first packet is received the sleep duty cycle of the chip is probably altered to less sleeping to make sure for a quick update on a new color.  
The user tracking must also be implemented on the 915 MHz radio. The chip periodical sends out a beacon containing the user's UID, this beacon is then received by base stations scattered around the festival site. With RSSI info and triangulation the user can then be located in a certain area. One of the interesting things to investigate would be to see how this system scales. When 18000 bracelets all send out beacons at the same time, there are going to be a lot of collisions. Maybe someone from Sendrato could reveal to us what MAC protocol is being used?  
The last feature, the Facebook contact sharing, can't be implemented on the NFC radio since this one can't initiate communication. It's basically just a programmable tag. I see two ways that this feature could be implemented.

*	Possibility one would be that the power of the transmitted packet (containing the UID), when the button is pushed, is turned way down so it only reaches a couple meters, and thus is only received by nearby receivers. The other bracelet would then receive the UID and store it on the NFC chip, which is basically an EEPROM. When leaving the festival site all the gathered UIDs would then be read via NFC from the chip.
*	The second possible implementation would be to send out a marked (full-power) packet containing the UID whenever the button is pressed. The antennas spread out around the festival site would receive these marked packets. When looking at the incoming packets their timing and location information all matching packets and UIDs can be grouped and matched up. This option has again the possibility of not scaling very well, what's the chance of 2 groups of possible friends pushing the button on their bracelet under the same antenna at the same time?

A ballpark guess at the price of this thing (complete bracelet): between $8 and $9. Let me know if you think I'm close, or way off.

A lot more info could be extracted from the bracelet once I power it on, probe the I2C lines and sniff the wireless traffic.  
But, that's for a next post.

Some more pictures of the complete assembly (click for super-size!):

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-1.jpg)](/images/2014-11-17-TML-bracelet-teardown-1.jpg)

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-2.jpg)](/images/2014-11-17-TML-bracelet-teardown-2.jpg)

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-3.jpg)](/images/2014-11-17-TML-bracelet-teardown-3.jpg)

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-4.jpg)](/images/2014-11-17-TML-bracelet-teardown-4.jpg)



