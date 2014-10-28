---
layout: post
title: Train schedule LED panel
date: '2014-09-14T15:06:00+02:00'
tags: []
tumblr_url: http://jelm.be/post/97495903365/train-schedule-led-panel
comments: True
---
A while ago I made a train count-down LED display. It’s basically an LED panel that displays the next trains leaving (or arriving in) any Belgian train station.

<!--more-->

I had a couple of these RG Bicolor LED Dot Matrix Boards laying around. So after letting them collect some dust for a while, I bolted them together, paired them with a 5V PSU out of the recycle bin, and made a small PCB to hook them up to an FTDI board and arduino mini.
The LED panel is based on the well documented HT1632c LED driver, so interfacing them with the arduino wasn’t that hard. The arduino is connected via the FTDI board over USB to a pc running a node.js script.
The script uses the irail.be api to pull in the json data of the next two trains leaving a Belgian station of choice. The data is then send as CSV over the serial output to the arduino. The arduino in turn parses the data and displays it on the LED panels.
The panel shows the departure time of the next train in yellow, its destination in green, the platform number in yellow and the possible delay in red.
The whole setup worked pretty well, but since I don’t take the train on a daily basis I never actually bothered mounting the display to a wall. If I would install it somewhere, I’d probably have the node script running on a cheap, but capable, TL-WR703N wireless router running OpenWrt.
I’m definitely not the first one making a display like this, here is one for the NYC subway for example.
Some more pictures of the display:



