---
layout: post
title: IoT IP camera teardown and getting root password
thumbnail: /images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-1.png
description: Opening up a cheap pan/tilt IP camera, inspecting its IP traffic, hooking up a serial console to the SoC and getting the root password for telnet login.
tags:
- IoT
- ipcamera
- ipcam
- openwrt
- teardown
- root
- telnet
- PTZ camera
- logilink
- Apexis
---

<style type="text/css">
  .gist
  .gist-file
  .gist-data {max-height: 300px}
</style>

This post will describe how I inspected the IP traffic of a cheap pan/tilt IP camera. Then continued to open the camera up, connect to the serial console of the SoC; extracted the root password and logged in via telnet over the wireless interface. My goal was to have a look at the security of these very cheap IoT devices, and see how they could be improved.

Let's have a look at our victim:

I present to you the [Logilink WC0030A](http://logilink.com/Products_LogiLink/Active_Network_Components/IP_Cameras/WLAN_Pan_amp;_Tilt_IP_Camera-10x_IR_LEDs_amp;_Microphone_WC0030A.htm) also known as the [Apexis APM-JP8015-WS](http://www.apexis.com.cn/productsdetails_220.html)

[![Logilink WC0030A](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-1.png)](/2016/03/14/IoT-IP-camera-teardown-and-getting-root-password/){: style="text-align: center;"}

<!--more-->

IP Cameras have become extremely cheap in the last couple of years. Mass production made the prices of decent quality image sensors, and very capable SoCs, drop significantly. Our victim in question is currently still available for [about €43](http://www.pollin.de/shop/dt/MTY3NjcyOTk-/Computer_und_Zubehoer/Netzwerktechnik/IP_Kameras/IP_Kamera_LOGILINK_WC0030A_Pan_Tilt_WLAN_schwarz.html), but clones/copies/similar models are available from China [for even cheaper](http://www.aliexpress.com/w/wholesale-ipcamera.html?site=glo&SortType=price_asc&SearchText=ipcamera&CatId=301102).

The cheap price is great if you want a cheap alarm system or an easy way to spy on your neighbors/pets, but it also means that the included firmware and software isn't very well developed (you get what you pay for). Accompanying apps are buggy, features are lacking, the firmware is buggy and tends to crash and the security was an afterthought.

Security in cheap IoT devices is becoming a big issue. Manufacturers don't really care about the personal data belonging to the users of their products, and the users themselves don't have the technical knowledge to asses the security of the device or to secure it (which sometimes means to just not use it).

As a quick example you can have a look at this simple [Shodan search query](https://www.shodan.io/search?query=port%3A554+has_screenshot%3Atrue) (Shodan is a search engine for the Internet of things, or basically it indexes everything Google doesn't), and be amazed at how many shops, living rooms, playgrounds, parking lots, kitchens, stairwells, gardens, factories, bedrooms (???), classrooms, pools, hotels and even the mourning-hall of a funeral home, have an unsecured live video feed for you to stare at.

Before I was going to use the above mentioned IPcam I wanted to have a look at how much data it leaked and to whom, and how hard it would be for someone to hijack the video feed and get a live view of my dog. (I was going to use this camera to remotely witness my puppy destroying my living room)



The Logilink [Logilink WC0030A](http://logilink.com/Products_LogiLink/Active_Network_Components/IP_Cameras/WLAN_Pan_amp;_Tilt_IP_Camera-10x_IR_LEDs_amp;_Microphone_WC0030A.htm) has a 0.3 MP sensor, a wired ethernet interface, a WiFi radio (wired and WiFi can't be used at the same time), some IR LEDs up front, 2 way audio, it can pan and tilt and has a trigger input and output (for alarm type things). All-in-all a fairly standard (low resolution) IP camera.

The camera comes with a web-interface accessible through a browser on its build in web-server and a seems to be compatible with a plethora of mobile apps that all come in a different flavor of buggy. The manual mentions two different logins in two different sections (admin:000000 and admin:1234), trying them at random at all the different login prompts seems to yield the best results.

[![Logilink WC0030A](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-2-small.png)](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-2-big.png)[![Logilink WC0030A](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-3-small.png)](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-3-big.png)

The web interface has your standard buttons and shows the device firmware- and web interface version (it's not the original web interface, I had reflashed it by this point with the Apexis one). I also don't own a purple couch, the white balance on this thing is horribly off. You can configure a custom dynamic DNS, but even if you do, or if you disable it, it seems that the camera always connects to the built in ddns-server (oipcam.com). No way of disabling that. The direct [MJPEG](https://en.wikipedia.org/wiki/Motion_JPEG) live-stream seems to be available at `http://[IP]/videostream.cgi?usr=[USERNAME]&pwd=[PASSWORD]`

Next steps will be to try to login to the camera, get a terminal, see what OS it's running and check what data is getting send to where.

Let's do a quick port scan of the camera to see what services it runs:
{% highlight bash %}
$ nmap [IP]

Starting Nmap 6.40-2 ( http://nmap.org )
Nmap scan report for [IP]
Host is up (0.012s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
23/tcp open  telnet
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 3.10 seconds
{% endhighlight %}

Port 80 was to be expected, telnet is a little more worrisome. Lets try to login with the standard passwords:
{% highlight bash %}
$ telnet [IP]
Trying [IP]...
Connected to [IP].
Escape character is '^]'.

(none) login: admin
Password: 1234
Login incorrect
(none) login: admin
Password: 000000
Login incorrect
(none) login: Connection closed by foreign host.
{% endhighlight %}

At least there is a different password on the telnet login than the ones mentioned in the manual. I guess that's something. Next step would be to go ahead and start brute forcing the telnet login, but lets first have a look at the different outside services the camera is connecting too (and what passwords it uses for those).

If we want to monitor the traffic from and to the camera, [Wireshark](https://www.wireshark.org/) is probably the best tool. We can't run it on the camera itself of course, so we'll have to intercept the packets on the router the camera connects to. I'm using a [Nexx WT3020](http://www.ebay.com/sch/i.html?&_nkw=Nexx+WT3020&LH_BIN=1) which becomes a very cheap, capable little router when you re-flash it with [OpenWRT](https://openwrt.org/). Ideal for snooping on traffic of connected hosts.

We'll just SSH into the router and use [tcpdump](http://www.tcpdump.org/) to print all the packets, which we then pipe through to a local instance of Wireshark. You can just install tcpdump-mini via the LuCI (the openWRT web interface) or via SSH:
{% highlight bash %}
root@OpenWrt:~# opkg update
root@OpenWrt:~# opkg install tcpdump-mini
{% endhighlight %}

Next we connect to the router, start a packet dump on the `br-lan` interface, exclude our SSH port, and send it to Wireshark
{% highlight bash %}
ssh -x root@192.168.1.1 tcpdump 'not tcp port 22' -i br-lan -s0 -U -w - | wireshark -k -i -
{% endhighlight %}
Or we can also just dump it into a .pcap file and dissect it later.
{% highlight bash %}
ssh -x root@192.168.1.1 tcpdump 'not tcp port 22' -i br-lan -s0 -U -w - > dump_$(date +'%T_%m-%d-%y').pcap
{% endhighlight %}
After connecting the camera and letting it go through its boot-cycle, lets open the capture file in Wireshark and start filtering:
{% highlight bash %}
eth.src == 54:cd:ee:[MAC] || eth.dst == 54:cd:ee:[MAC]
{% endhighlight %}
This filter gives you all the packets going in and out of the camera based on the camera's MAC address:
[![Wireshark capture](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-4-small.png)](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-4-big.png)

We can see the camera making a standard DHCP request first, after getting an IP from the router, the camera sends a DNS request for `time.nuri.net` and `checkip.dyndns.org`. The first one is an [NTP](https://en.wikipedia.org/wiki/Network_Time_Protocol) server (somewhere in China), probably used to set the clock of the camera. The second URL is used to get the outward facing IP of the camera via dyndns.org.

Then the camera continues to make a DNS request for `oipcam.com`, and sends the following HTTP request to it: 
{% highlight bash %}
http://www.oipcam.com/vipddns/upgengxin.asp?username=o9428&userpwd=958&userdomain=o9428.oipcam.com&userport=80&mac=00-00-00-00-00-00
{% endhighlight %}
(I changed user and pass here, MAC is all zeroes though). oipcam.com answers with a HTTP 200 OK packet with contents `UP`.

Next DNS request is for `www.apexisalarm.com`, with an HTTP request to
{% highlight bash %}
http://www.apexisalarm.com/apns.php?cmd=reg_server&uid=53XHWU68T345HGf571N0
{% endhighlight %}
(Again I changed uid). Which returns the following answer: `reg_server !!!!<br>Server device 53XHWU68T345HGf571N0 login.`

After which the camera pings the router, probably to check if the network is still up.

Another DNS request for `checkip.dyndns.org`, another 2 pings to the router, a DNS request for `www.3322.org` with HTTP request `http://www.3322.org/dyndns/getip`. This thing really needs to figure out its IP.

Several DNS request for `www.ip138.com`, which all return failures. And eventually the camera loops while sending ping requests to the router.

So it seems the camera uses 2 outside services located at `oipcam.com` and `apexisalarm.com`. The other requests are for time and external IP (these could leak your IP too of course).

Trying any of the usernames and passwords found in these HTTP requests in the telnet login doesn't get us any further either.



Next step is to open this thing up, and see what makes it tick!

[![Logilink WC0030A](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-5-small.jpg)](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-5-big.jpg)

Ports on the back of the camera, note how the silkscreen for the alarm inputs is a little more clear than the markings on the casing.

[![Logilink WC0030A](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-6-small.jpg)](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-6-big.jpg)

Bottom of main PCB, nothing really interesting here, note the reset button on the right, the unpopulated QFP and header on the left and microphone at the top.

[![Logilink WC0030A](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-7-small.jpg)](/images/2016-03-11-IoT-IP-camera-teardown-and-getting-root-password-7-big.jpg)

Inside we can see that the SoC controlling the camera is the `Ralink RT5350F` on what seems to look like an OEM module plugged into the main PCB as break-out board. The `JP8015` marking seems to be a reference to the Apexis model number of the camera. We can also see that the movement is controller by two 5V DC stepper motors.

This Ralink chip is pretty capable, and is the heart of some smaller cheap WiFi routers, most of them running OpenWRT. It runs Linux at 360 MHz, has 24 GPIO pins, 8 MB storage, 32 MB RAM. This particular OEM module seems to resemble the [NixCore X1](http://nixcores.com/nixcore_x1.php) a lot, but it's not quite the same. Let's hope the pinout of the main header matches though.

Having a look at [this](http://nixcores.com/img/x1/nixcorex1_header-1.0.png) table, and the documentation for the X1, we should be able to get a serial terminal on RX2 and TX2, which is pin 39 and 40. If we have a look at the camera, those two pins are there on our module, but aren't populated. Looks like a good sign.

After connecting a 3.3V serial USB converter to the module at 5670057600 baud, we're in!

The resulting bootlog:

<script src="https://gist.github.com/JelmerT/530871a9731370930db8.js"></script>

Lots of interesting stuff here, but let's focus on trying to get into the camera via telnet. We can just:

{% highlight bash %}
$ cat /etc/passwd
root:1naesbIMqm.cY:0:0:root:/:/bin/sh
{% endhighlight %}

This shows us there's only one user, `root` with password `1naesbIMqm.cY`. This is of course not the actual password, but the password encrypted with a one-way [DES](https://en.wikipedia.org/wiki/Data_Encryption_Standard) algorithm.

Let's try brute-forcing it! I'm using [John the ripper](http://www.openwall.com/john/):
{% highlight bash %}
$ john passwd --show
root:123456:0:0:root:/:/bin/sh

1 password hash cracked, 0 left
{% endhighlight %}
Well that was easy.

Let's try `123456` as login on the telnet prompt:

{% highlight bash %}
$ telnet 192.168.1.242
Trying 192.168.1.242...
Connected to 192.168.1.242.
Escape character is '^]'.

(none) login: root
Password: 123456


BusyBox v1.12.1 (2012-11-19 22:34:42 PST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

# 
{% endhighlight %}

And we're in! (For the second time).

{% highlight bash %}
# cat /proc/version 
Linux version 2.6.21 (root@test) (gcc version 3.4.2) #136 Mon May 20 11:39:34 CST 2013
# cat /proc/cpuinfo 
system type             : Ralink SoC
processor               : 0
cpu model               : MIPS 24K V4.12
BogoMIPS                : 239.61
wait instruction        : yes
microsecond timers      : yes
tlb_entries             : 32
extra interrupt vector  : yes
hardware watchpoint     : yes
ASEs implemented        : mips16 dsp
VCED exceptions         : not available
VCEI exceptions         : not available
{% endhighlight %}

Now we can start poking around the file system. `test.sh` seems to be doing most of the work and starts the 2 main processes:
{% highlight bash %}
...
videocatch>/dev/null 2>&1 &
ipcamn>/dev/null 2>&1 &
...
{% endhighlight %}

Killing any of these processes makes the watchdog timer run out and resets the system.

Looking at the loaded kernel modules:
{% highlight bash %}
# lsmod
Module                  Size  Used by    Tainted: P  
usbvideo 56752 1 - Live 0xc0116000
watchdog 2064 1 - Live 0xc012d000
gpio 3968 3 - Live 0xc012b000
i2s 10688 3 - Live 0xc0140000
i2c 2816 0 - Live 0xc008a000
rt2860v2_sta 1319712 1 - Live 0xc0265000 (P)
{% endhighlight %}

It seems we're using the I2S interface for sound, [usbvideo or UVC](https://en.wikipedia.org/wiki/USB_video_device_class) for the cheap Alcor Micro Corp USB camera (058f:3861). I2C, probably for the stepper controllers and gpio for the alarm input / outputs and the LED.

If we take a quick look at `videocatch` by dumping all the strings in the executable:

<script src="https://gist.github.com/JelmerT/699a6897741d47a4fdda.js"></script>

We can see that this process does the init of the USB camera, and seems to deal with the video.

Dumping the strings in the `ipcamn` process:

<script src="https://gist.github.com/JelmerT/da0b5e2c242ba38f7ee7.js"></script>

Ipcamn seems to be dealing with all the rest of what makes the camera and its services run.

At this point we have a good idea on what services the camera connects to, and what ports it opens. We could go further and take apart the `ipcamn` executable. But with a telnet login of `root` and `123456` , I kind of don't want to know what's in there.

My current solution is to block all traffic going to and from the camera and the outside at router level. And having a video server like [motionEyeOS](https://github.com/ccrisan/motioneyeos/wiki) or [ZoneMinder](https://zoneminder.com/) do the heavy lifting of recording, storing and streaming video.

The ideal solution to this problem would be to compile a custom build of OpenWrt and flash it to the camera. And of course getting the USB webcam and all other I/O devices to work. This way you'd have full control of the camera and all its data. As OpenWRT and DD-WRT can transform shady cheap WiFi routers into a solid, stable piece of networking equipment, a version of these OSes for IP cameras could transform them into a cheap, stable and secure way of doing video surveillance. I call it: CamWrt or OpenCamWrt or OpenCam ... not entirely sure yet. That's for a future post.

If you want any more info on any part of this teardown, or noticed some mistakes, don't hesitate to contact me or leave a comment!
