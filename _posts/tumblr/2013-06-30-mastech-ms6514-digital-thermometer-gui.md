---
layout: post
title: Mastech MS6514 Digital Thermometer GUI
tags:
- mastech
- MS6514
tumblr_url: http://jelm.be/post/54268187548/mastech-ms6514-digital-thermometer-gui
comments: True
---
![MS6514](/images/2013-06-30-mastech-ms6514-digital-thermometer-gui-1.jpg)
{: style="text-align: center;"}

This was a small in-between project for the [Mastech MS6514](http://www.p-mastech.com/index.php?page=shop.product_details&flypage=flypage.tpl&product_id=150&category_id=18&option=com_virtuemart&Itemid=29&vmcchk=1&Itemid=29) dual channel digital thermometer. (It will probably work for the MS6513 too, maybe with some small changes.)

<!--more-->

My Dad needed an easy hand held thermometer for cooking and brewing that can also do some basic logging. The MS6514 seemed like a good candidate since it had 2 channels and looked quite sturdy for a (fairly) cheap Chinese build device. After looking for the best price we ordered one from [Goodluckbuy](http://www.goodluckbuy.com/) (worst online shop name ever btw).

After some weeks my dad received and tested the meter with some decent ebay [K-type thermocouples](http://en.wikipedia.org/wiki/Thermocouple). The meter seemed to work pretty well, but the logging was kind of a mystery feature. The meter has a usb port and came with some software on one of those [tiny half sized cd's](https://en.wikipedia.org/wiki/Mini_CD) that no recent laptop can actually read..

Out came the hacking skills: the meter seemed to use a simple serial connection over usb to communicate. It clearly used the [CP2102](http://www.silabs.com/products/interface/usbtouart/Pages/usb-to-uart-bridge.aspx). After some reverse engineering I sort of had a clue how the protocol worked. Based on this I made a quick and dirty GUI in Processing that displays all the info found on the display of the device and gives a basic graph of the past measurements.

![MS6514](/images/2013-06-30-mastech-ms6514-digital-thermometer-gui-2.png)
{: style="text-align: center;"}

Just select the correct serial port, make sure you long-push the “PC-Link” button and the serial strings should start coming in. There is also some exporting love inside to export measurements to a spreadsheet and graph brewing cycles or just outside and inside temperature (of that turkey in the oven).

All code is up on [Github](https://github.com/JelmerT/MS6514GUI), all is functional, but beware it could be a bit buggy. Don’t rely too much on the timing either, the meter seems to spit out it’s measurements at kind of random moments. The graph is also a pretty rude implementation. There is a lot of room for improvement, but this was workable for us.

Fork it and improve!
