---
layout: post
title: Interactive and connected foosball table
thumbnail: /images/2014-10-29-Interactive-and-connected-foosball-table-1.jpg
description: Writeup of an interactive and connected foosball table.
tags:
- beyond.io
- PCB design
- foosball
- iot
---


Around October 2013 I helped out [beyond.io](http://www.beyond.io) with their interactive and connected foosball-table project.


<div class="post-videoWrapper"><iframe src="https://player.vimeo.com/video/76803848?title=0&byline=0&portrait=0" width="740" height="425" frameborder="0" title="Urban Crafts Kicker" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

<!--more-->

Video by [evilminions.be](http://www.evilminions.be/)

This project, sponsored by a big Belgian beer brand (PALM), consisted of around 20 interactive foosball tables being placed at several Belgian youth houses. Every foosball table is Internet connected and keeps scores of each game that's being played. As a team of foosball players, you can compete, via this table, in a local foosball table championship. Every player gets an RFID key chain to identify her or himself to the table, and system, before every game. On the online portal you can then check your live scores and see where you and your team is placed in the ranking. The winning team goes home with a lot of free beer of course.

The table itself has some nice features like LED lighting of the playing field, dual LED matrix displays for feedback, big arcade style back-lit buttons for the users to push (read: punch) on, and even some strobe lights underneath the table used for key moments in the game.

Since this was a pretty small production run with a fairly short development time, we went for a [raspberryPi](http://www.raspberrypi.org/) as the main controller to handle all Internet connection related tasks, and an Arduino Leonardo clone for all real-time game related tasks. We aptly named our clone `Arduino Ronaldo`.

![Foosball table](/images/2014-10-29-Interactive-and-connected-foosball-table-1.jpg)

I mainly took care of the electronic design, defining the BOM and helped the project get ready for production.

The first PCB design was fully functional (in green), I love when that happens, and the second was the production version with some small improvements (in black).

![Foosball table](/images/2014-10-29-Interactive-and-connected-foosball-table-3.jpg)

![Foosball table](/images/2014-10-29-Interactive-and-connected-foosball-table-4.jpg)

The Atmel based controller handles all the real-time inputs and outputs, like goal switches, user buttons and shot-force sensors. But also the LED-matrix display, strobe- and field lighting. It's connected over USB to the raspberryPi who handles the wired or wireless Internet connection and the dual USB RFID readers.

All boards were populated with our, sometimes somewhat picky, [TM220A](http://www.neodentech.com/bbx/996360-996360.html?id=26798&pid=761184).

<div class="post-videoWrapper"><iframe src="https://player.vimeo.com/video/74738858?title=0&byline=0&portrait=0" width="740" height="425" frameborder="0" title="Urban Crafts Kicker" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

<br />

![Foosball table](/images/2014-10-29-Interactive-and-connected-foosball-table-5.jpg)

After flashing and testing each individual board, we set up a small local production line to retrofit the existing new foosball tables with all the electronics.

![Foosball table](/images/2014-10-29-Interactive-and-connected-foosball-table-2.jpg)

Once all the back-end code was working, all tables were deployed to their individual locations. Everything seemed to work as planned, except for that single odd connector we forgot to hot-glue down off course. These tables get a pretty thorough vibration test by their players once the game starts heating up!

The tables all still work up until this date, so I'd name this a successful project.

For more info and other similar projects, have a look at [beyond.io](http://www.beyond.io).

<!-- Client: [urbancrafts.tv](http://urbancrafts.tv) (PALM) which is a concept of [trendwolves.com](http://trendwolves.com) -->
