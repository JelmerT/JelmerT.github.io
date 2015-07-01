---
layout: post
title: Difference between CC2630 and CC2650
tags:
- TI
- CC2650
- CC2630
- IOT
- teardown
- decap
---
Texas Instruments released a new [family of ultra-low power wireless MCUs](http://www.ti.com/lit/wp/swry014/swry014.pdf) (CC26XX and CC1310). They come in 3 different (physical) sizes and in 5 different flavors of supported wireless protocols. There is the [CC2620](http://www.ti.com/lit/ds/symlink/cc2620.pdf) which supports RF4CE, the [CC2630](http://www.ti.com/lit/ds/symlink/cc2630.pdf) for Zigbee and 6LoWPAN, the [CC2640](http://www.ti.com/lit/ds/symlink/cc2640.pdf) for Bluetooth Smart and the [CC2650](http://www.ti.com/lit/ds/symlink/cc2650.pdf) which does all the 2.4GHz based protocols. The [CC1310](http://www.ti.com/lit/ds/symlink/cc1310.pdf) takes care of all the sub-GHz protocols, so we won't be looking at that one in this comparison.

The price difference between the different models is quite significant: the CC2630, 6LoWPAN model currently sells for an average of [$11.87](https://octopart.com/cc2630f128rgzt-texas+instruments-56243165) while the CC2650, multi protocol version currently goes for [$15.03](https://octopart.com/cc2650f128rgzt-texas+instruments-56243167).  
So how much hardware difference is there really between these models? Do you pay for different silicon, or are these different flavors more similar than their markings let on?

Only one way to find out!

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-23-small.jpg)](/2015/06/30/Difference-between-CC2630-and-CC2650/)

<!--more-->

I sampled both the CC2630 and CC2650 (Thanks TI!), decapped both chips and compared the dies.

How to decap ICs:

I like to use fuming nitric acid ([HNO<sub>3</sub>](https://en.wikipedia.org/wiki/Nitric_acid#Fuming_nitric_acid)), the process goes pretty quick and the die comes out nice and clean, ready for the microscope.

**DISCLAMER: HNO<sub>3</sub> is extremely dangerous.  
Like, really, dude.  
Only replicate this if you know what you're doing and have the right tools to handle highly corrosive acids and chemicals. This stuff will decompose your skin and flesh. Always wear gloves, eye protection, lab coat and use a fume hood.**

If you're not trying to save any of the bonding wires in the chip, the process is pretty easy. Just take some nitric acid, throw the chip in there. The acid will start reacting with the packaging and you'll see it bubble, dissolve and delaminate. To make the process go a little faster you can add some heat to the reaction by putting the whole thing on a hot plate. Make sure you keep an eye on the die and rinse it off with acetone in time to make sure the acid doesn't damage the die itself.  
The speed of the process depends on the size of the package, the amount of metal / pins in there and of course the purity of the HNO<sub>3</sub> and added heat. For this chip it took me about 5 to 10 minutes.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-2-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-2-big.jpg)

Gather all your tools and some (sampled) chips in the fume hood.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-1-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-1-big.jpg)

Fuming HNO<sub>3</sub> is not the easiest to get if you're not a chemistry- or university lab. Depending on where you live of course.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-3-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-3-big.jpg)

Five tries each, should do it.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-4-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-4-small.jpg)

CC2630 on the left, CC2650 on the right.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-5-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-5-big.jpg)

Submerge the chip in the HNO<sub>3</sub>.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-6-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-6-big.jpg)

Observe the reaction.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-7-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-7-big.jpg)

The metal on the chip will start gently bubbling and dissolving while the rest of the packaging starts crumbling off.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-8-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-8-big.jpg)

The HNO<sub>3</sub> turns greener the further the reaction goes.

<div class="post-videoWrapper"><iframe src="//player.vimeo.com/video/132253489?title=0&byline=0&portrait=0" width="740" height="425" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

By gently heating up the HNO<sub>3</sub> you can make the reaction go slightly faster.

<div class="post-videoWrapper"><iframe src="//player.vimeo.com/video/132253490?title=0&byline=0&portrait=0" width="740" height="425" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

The packaging will start to delaminate and the bare die will become visible. With some practice you can get the chip out just before the bonding wires dissolve.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-10-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-10-big.jpg)

Take the chip out of the HNO<sub>3</sub> and clean it of with acetone.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-11-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-11-big.jpg)

The packaging will further crumble off.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-12-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-12-big.jpg)

The back of the die is now clearly visible and the packaging is almost completely dissolved.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-13-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-13-big.jpg)

Keep the die in the HNO<sub>3</sub> until the packaging is completely gone.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-14-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-14-big.jpg)

To inspect the die itself you do need a proper microscope. The needed magnification of course depends on the scale of the manufacturing process of the die. Having a proper X-Y table will also help a lot when staring at the silicon.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-15-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-15-big.jpg)

I couldn't get the camera on my microscope to properly work, because of drivers and Windows (Damn you Windows!). So the actual pictures of the die are made with the 'point your cellphone cam through the eyepiece technique'.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-16-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-16-big.jpg)

Top of the die is already visible, but it needs some more time in the HNO<sub>3</sub>.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-17-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-17-big.jpg)

Almost there!

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-18-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-18-big.jpg)

Close up of the die. Pretty sure this is part of the radio on the chip. I'm guessing some sort of coil? Please correct me if I'm wrong. At the top you can see the pads where the bonding wires used to be connected.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-19-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-19-big.jpg)

This is a die shot of the CC2630, I did get a little too rough while cleaning and made some scratches.

Next step in this comparison is to look for the die markings of both ICs and compare them.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-20-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-20-big.jpg)

That looks like part of the TI logo, but of course the rest of the marking broke off. I'm not sure if the die was packaged like this, or if I broke of this corner. Big chance I did though.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-21-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-21-big.jpg)

After decapping another CC2630 I got this result! Very clear marking on the die: **2014TI F771735**

Now for the comparison I went through the whole process again with a CC2650. Which gives ...

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-22-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-22-big.jpg)

Indeed, the same **F771735** marking!

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-23-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-23-big.jpg)

Both dies next to each other on my finger for size.

So it seems that both the CC2630 and CC2650 have the same die markings and thus are identical on silicon level. I'm guessing that the CC2620 and CC2640 also have an identical die ([Chipworks](https://chipworks.secure.force.com/catalog/ProductDetails?sku=TEX-CC2640F128RGZR&viewState=DetailView) actually agrees with me on the CC2640). This could mean that the differences in supported protocols is purely based on which firmware TI programs into the chip. Making the CC26XX series chips a form of [crippleware](https://en.wikipedia.org/wiki/Crippleware).

All radio tasks in the CC26XX chips are being handled by the ARM Cortex-M0 co-processor. So my best guess is that this processor just runs different firmware based on the version of the chip. Reflashing this firmware is blocked, except in the more expensive 'multi protocol' CC2650.

It would be interesting to see what measures TI implemented to prevent reflashing of this radio firmware in the fixed protocol models, and if there is a way to circumvent them.


But I'll leave that as an exercise for the reader ;) (But do report here if you have some more info!)

<br>
<br>

**BONUS:** Some more die shots I made of a CC2531 a while ago. This die is made with a bigger scale manufacturing process, so the die itself is bigger and the pictures are a little clearer.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-24-small.jpg)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-24-big.jpg)

Comparison between the older brother, CC2531 on the left (with some bonding wires still attached) and the CC2650 on the right.

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-25-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-25-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-26-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-26-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-27-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-27-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-28-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-28-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-29-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-29-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-30-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-30-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-31-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-31-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-32-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-32-big.png)

[![CC2630 vs CC2650](/images/2015-06-30-Difference-between-CC2630-and-CC2650-33-small.png)](/images/2015-06-30-Difference-between-CC2630-and-CC2650-33-big.png)