---
layout: post
title: Custom shaped video wall for Sound Of Stereo
date: '2014-09-14T00:46:00+02:00'
tags: []
tumblr_url: http://jelm.be/post/97446451245/custom-shaped-video-wall-for-sound-of-stereo
comments: True
---
This is a project I finished a while ago (festival season 2012) but I never got around to doing a proper write-up. (See end of post for the movies of the end result)





End 2011 Sound of Stereo, a Belgian dj-duo, approached me with an idea for the show of their next festival / club tour through Belgium / Europe. We started working out the idea together and ended up doing a great live-set tour. It involved a custom led-wall shaped like the Sound Of Stereo logo running video, perfectly synced to some great new songs.
The basic set-up of the show consisted of: 2 dj’s, about 10 moving heads, some strobes, some conventional blinders and 3 custom shaped led walls on stands. The whole show was controlled by a GrandMA 2 and a video server running Madrix. Everything was synced and triggered by midi triggers programmed into the songs of the set playing on a Macbook on stage.


Depending on the venue this set-up was of course accompanied by the local house lights or expanded with some more movers and blinders.
The teaser from the live set gives a pretty good impression of what the set-up looked like.

Programming all the moving lights, blinders and strobes was fairly easy, getting a custom LED wall in the shape of the SOS logo, not so much.
We played with the idea of integrating LEDs into the show to give it an extra dimension, but soon noticed that having a couple DMX controlled LED strips wasn’t going to cut it. The idea spun out of control and we ended up with plans for 3 individual 2 meter wide CNC’ed frames with about a thousand individually controllable RGB LEDs mounted on them in the shape of the logo for the tour.



This plan allowed us to display video behind the dj’s during the show but not have it obstruct any other luminaires that originate even further back on stage, which creates a sense of depth that you usually lose when working with non transparent video walls on stage. The show was a live set, which means the dj’s could play any song at any time in any which way they wanted (there was a fixed set list though), but all the lighting effects and video still needed to be perfectly synced to the music. This meant that we needed full control over the whole set up at all times, which results in controlling every single pixel individually with the help of a video server.
After drawing up all the plans and calculating all the constraints (Can we make this? Can we transport this? Can we move this on and off stage quickly enough? Can we control this? Can this be powered in every venue? Is the fps of the video high enough? …), we started of by ordering all the parts and CNCing the frames at our local fablab (FablabXL).


For the LEDs themselves we needed individually controllable LED strips that we could easily cut to the correct size. We also needed full control of the exact RGB value for every LED, preferably over DMX, since we wanted to be able to program them together with all the other lighting effects. Sending an actual video signal, in some way, to the strips was also an option, but would become pretty complicated when doing the mapping in the video server.

We opted for the LPD8806 since this chip offers actual 7-bit PWM for every channel, which means sort of good RGB control of every individual LED for video. The cheaper and more popular HL1606 chip at the time didn’t do any PWM, which would have resulted in pretty ugly video colors. The individual LEDs on these strips are about 3 cm apart which resulted in an acceptable pixel spacing.
The downside of these strips was that it’s controlled by an SPI-like protocol. The documentation of the obscure Chinese chip was pretty scarce, but Adafruit did have some libraries to control them, so it looked feasible. The strips Adafruit carries are also ‘weatherproof’, which is great in an environment where flying beer and champagne fountains aren’t unheard of.
After ordering some reels of LED strip the real work started. How to control about a thousand LDP8806 RGB LEDs over DMX? (with an at least 30fps refresh rate)
Since one DMX universe holds a maximum of 512 different channels, and we have more or less 1000 LEDs with 3 channels (yes that’s 3000 dmx channels) we’d need about 6 separate universes to control the LED wall. This would mean having 6 separate ‘DMX cables’ (twisted-pair data cable with 5 pin XLR connectors) going to our LED wall. Which is no problem for our GrandMA2 lighting desk, but not my idea of ‘fun’. We needed to be able to move the whole setup on and off stage in a couple of minutes, and having less cabling is always better.
In comes Art-Net, this ethernet based protocol allows you to send packeted DMX data over a standard UTP cable. It sort of maxes out at 32,768 universes on a fast network and uses standard switches and other network gear. Seems like perfect fit for our LED wall.
Next step was to build a controller for the LED wall that takes Art-Net in and spits out the correct spi-like protocol for all the led strips. There were some good libraries available for Arduino, but moving so much data at a (semi) high speed was a little much for a poor 8-bit micro, and ethernet on Arduino is a no-no in my world. (this was before the Arduino Yun came out)
An mbed seemed like a perfect fit, since it has on board ethernet and is plenty fast to spit out some custom spi. The downside was that there were no working libraries for the LDP8806, the upside was there was an Art-Net library that sort of kinda worked.
After ordering some mbeds, I ported the Adafruit Arduino library for the LDP8806 to the mbed (Library here). This was of course only the first step. The hardest part was of course to get the existing Art-Net library to receive Art-Net packets and correctly map the DMX content to the correct LEDs.

After a couple of late nights with wireshark by my side and way too many leds burning out my retinas every time I’d send the wrong packet, I got 3 separate controllers finished. Each letter of the logo was to get its own dedicated controller.





Each controller consisted of the dedicated mbed and a beefy 5v supply to power that part of the LED wall. I opted for neutrik ethercon and powercon connectors, since these are strong, industry standard, and can be connected by any (less tech savvy) stage hand. Everything was mounted in a strong water-tight PVC case, with an extra copper mounting plate to dissipate any possible heat coming from the PSU (wasn’t really needed though).
Next step was to mount all individual pieces of LED strip on the CNC’ed frame and solder everything together. Not the most fun job, but hey, you wanted a custom shaped LED wall …




Some mounting points and extra black paint to touch up later we had a working custom LED display.



Hauling all of this around to multiple clubs and outdoor festivals of course meant we needed a/multiple proper flight case(s). At this point we were kind of running out of time, so the case was just made out of plywood instead of decent durable wood. But it worked for the whole duration of the tour. As inserts for the cases I used simple styrofoam with CNC’ed cut outs for all the stuff that needed to fit.





The flight-case has different levels, with in each level a different letter and all its cables, controller, and brackets. This was the easiest way to transport the three quite big and weirdly shaped frames.
After all the hardware was done and working came the next step: programming the actual one hour long show. 


We spent a couple of weekends repeating every song in the live set over and over, and tweaking every cue until it was just right.
The control setup looked something like this:

Yes, that is probably the worst schematic I’ve ever drawn, but it got the message across. (to the house light techs and stage hands)
On stage we had the three letters with each their dedicated controller, these were connected to a gigabit switch with UTP. There was also an Art-Net to DMX converter connected to the same switch. One single UTP cable runs all the way to the FOH where I had the GrandMA2 and the Madrix server running on a Macbook Pro (running windows :( ). Both were connected to the FOH-switch. From the MIDI output of the Dj’s laptop (the one that actually plays the music) I had an extra twisted pair all the way to the GrandMA2.
The GrandMA2 sends about 2 dmx universes over Art-Net to all the moving lights and the Madrix video server sends, on the same network, another 8 universes to the LED wall. Having everything on the same network meant I could monitor the DMX output of the Madrix server on the GrandMA2 and even override if I wanted to.
The Djs send triggers over MIDI to the lighting desk, which in turn starts the correct cue. The cue then controls the moving lights on stage and the Madrix server in the FOH (via dmx in over Art-Net). The video server then starts the correct pixel mapped video clip and sends out the dmx values over to the LED controllers on stage.
It took a while to program the whole show this way, but the end results was that I could sit back and relax once the whole system was set-up. All effects and video were automatically triggered to the currently playing song.

The whole setup performed pretty well throughout the whole tour and got expanded and stripped down for differently sized venues. If I were to do it all over, I’d definitely want to find a way to send the MIDI signals over ethernet (without having an extra laptop) and maybe a way to make the LED wall frames foldable, so they’re a little easier to handle. There was one nasty bug in the controllers I never got to catch though (ran out of time): once in a while a controller would just freeze. Setting the watchdog on the mbed made it recover perfectly, but there still was this 2 secs of frozen video, which I of course noticed, but not many other people did.
Since this project was so custom, it doesn’t make much sense to post any of the code or design files (except for the LDP8806 library). But If anyone is interested, just contact me.
Big thank you to Vincent, Jochen and everyone else making the tour a success!







Some more movies of the end result:





