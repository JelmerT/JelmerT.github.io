---
layout: post
title: Mastech MS6514 Digital Thermometer GUI
date: '2013-06-30T12:51:00+02:00'
tags:
- mastech
- MS6514
tumblr_url: http://jelm.be/post/54268187548/mastech-ms6514-digital-thermometer-gui
comments: True
---


This was a small in-between project for the Mastech MS6514 dual channel digital thermometer. (It will probably work for the MS6513 too, maybe with some small changes.)

My dad needed an easy hand held thermometer for cooking and brewing that can also do some basic logging. The MS6514 seemed like a good candidate since it had 2 channels and looked quite sturdy for a (fairly) cheap Chinese build device. After looking for the best price we ordered one from Goodluckbuy (worst online shop name ever btw).
After some weeks my dad received and tested the meter with some decent ebay K-type thermocouples. The meter seemed to work pretty well, but the logging was kind of a mystery feature. The meter has a usb port and came with some software on one of those tiny half sized cd’s that no recent laptop can actually read..
Out came the hacking skills: the meter seemed to use a simple serial connection over usb to communicate. It clearly used the CP2102. After some reverse engineering I sort of had a clue how the protocol worked. Based on this I made a quick and dirty GUI in Processing that displays all the info found on the display of the device and gives a basic graph of the past measurements.


Just select the correct serial port, make sure you long-push the “PC-Link” button and the serial strings should start coming in. There is also some exporting love inside to export measurements to a spreadsheet and graph brewing cycles or just outside and inside temperature (of that turkey in the oven).
All code is up on Github, all is functional, but beware it could be a bit buggy. Don’t rely too much on the timing either, the meter seems to spit out it’s measurements at kind of random moments. The graph is also a pretty rude implementation. There is a lot of room for improvement, but this was workable for us.
Fork it and improve!
