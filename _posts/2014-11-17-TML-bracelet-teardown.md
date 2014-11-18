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

But the bracelet had a couple more functions. The leather band clearly has 2 RGB LEDs embedded on either side of the ##watch face##. These LEDs are remote controlled by the [FOH](http://en.wikipedia.org/wiki/Front_of_House) to light up in sync and with the correct color to match the light show. This is a pretty neat effect, especially when a couple thousand people are waving their hands in the air.

<iframe width="740" height="425" src="//www.youtube.com/embed/eDZaea7OCd0?rel=0" frameborder="0" allowfullscreen></iframe>

This effect has already been used before and was invented and commercialized by [Xylobands](http://www.xylobands.com/) ([original patent](https://www.google.co.uk/patents/WO2013021209A1)) and backed by Coldplay.

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-6.jpg)](/images/2014-11-17-TML-bracelet-teardown-6.jpg)
([source](http://www.tomorrowland.com/en/2014-w2))

Another function of the bracelet was to let visitors easily exchange Facebook details when making new friends at the festival. The bracelet has a button embedded in the strap (underneath the heart). When both guests push this button at the same time, they'll get some feedback via the LEDs and at the end of the festival an email appears in their inbox with the contact details of the other person.

![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-7.jpg)]

Yet another function of the bracelet, and maybe one that was less known to its users, is the ability to track people. The bracelets have a unique ID and a wireless radio embedded by which people can be identified, but also located in a certain area. This is done by setting up different antennas and then triangulating the signal coming from a bracelet or just looking at the [RSSI](http://en.wikipedia.org/wiki/Received_signal_strength_indication) at the different antennas.

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-8.jpg)](/images/2014-11-17-TML-bracelet-teardown-8.jpg)
([source](http://sendrato.com/?page_id=32))

I also heard that the bracelets could have the capability of being linked to your credit card, so it can be used as an electronic purse at the bars and food stands on the festival site.

All these different functions and capabilities got me wondering about what makes these bracelets tick (pun intended). Since the RFID function for entrance control and payment clearly needs a short range wireless radio and the LEDs and user tracking a long range radio, there must be at least 2 radios inside the bracelet. And of course 2 corresponding antennas. The festival lasted about 2 times 3 days, so battery might become a concern, especially if the LED function is being used often. And how does the exchanging of Facebook contacts work? Is there any storage on the bracelet to store IDs? All these functions seemed possible, but could become expensive (hardware-cost wise) and bulky. The ticket price for Tomorrowland isn't cheap (about €230), but I presume they still needed a cheap, mass produced piece of electronics.

Lets have a look what's inside!

video goes here

On the back of the bracelet we can clearly see the markings: the bracelet is a product of [**Sendrato**](http://sendrato.com/) and the model seems to be **SUM3**, which has to be custom designed for Tomorrowland (the casing anyway).  
At first glance it seems my guess about the bracelet having 2 radios on-board seems to be correct. The PCB inside the bracelet is composed of a central hard FR4 circular board (four layer). Two pieces of flex PCB, containing antennas, LEDS and the button, are soldered on both sides to the main board. It's interesting to see that they used separate flex parts and soldered / glued them to the main board as apposed to a complete [rigid-flex solution](http://www.candorind.com/assets/img/Flex-RigidMax800px500pxl.jpg) where the flex part is just a layer of the FR4 PCB. I guess the extra labor cost was cheaper than the manufacturing price of the more complicated PCB.

On the main PCB we can find 3 chips, a crystal and some resistors, caps and coils. The main processor is the Silicon Labs [SI1063-A-GM](http://www.silabs.com/Support%20Documents/TechnicalDocs/Si106x-8x.pdf), which is a pretty capable 8051 based [SoC](http://en.wikipedia.org/wiki/System_on_a_chip). 

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-1.jpg)](/images/2014-11-17-TML-bracelet-teardown-1.jpg)

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-2.jpg)](/images/2014-11-17-TML-bracelet-teardown-2.jpg)

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-3.jpg)](/images/2014-11-17-TML-bracelet-teardown-3.jpg)

[![Tomorrowland bracelet](/images/2014-11-17-TML-bracelet-teardown-4.jpg)](/images/2014-11-17-TML-bracelet-teardown-2.jpg)