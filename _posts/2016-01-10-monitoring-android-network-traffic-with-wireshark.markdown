---
comments: true
layout: post
title:  "Monitoring Android Network Traffic"
date:   2016-01-10
categories: android wireshark
---
*MacBook Air running Yosemite*  
*Nexus 5X running 6.0.1*  
*Ethernet connection*  
*Wireshark*  

Here's a quick way to analyze network traffic from a mobile device. I'm pretty sure this will work for Windows and Linux + iOS so long as you can follow these steps. There's no need to use Charles Web Debugging Proxy or root your Android device.

#Step 1: Tethering
I'm sure this can be generalized - the gist is that you want to use your computer as a router for your mobile device. On OS X this meant going to System Preferences > Sharing and turning on Internet Sharing with settings to share your ethernet connection to computers using WiFi. From here, connect your device to the WiFi endpoint you created.

#Step 2: Using Wireshark
[Download Wireshark](https://www.wireshark.org/download.html)  
Can we stop and appreciate the new version of Wireshark for Mac OS X? Look at those nifty little graphs next to each capture interface. Hopefully Wi-Fi: en0 looks active at this point. Double-click to start capturing packets. Okay now there may or may not be a seemingly endless stream of TCP/UDP network packets. Let's ignore those. Wireshark is incredibly powerful but all we care about is finding out our mobile device IP so that we can begin to filter out unecessary data from the stream. Add `http` to the filter bar and press enter. It sort of looks like a browser address bar if for some reason you don't know what I'm talking about. Great, now if you're familar with web this probably looks a lot more familiar, classic GET requests, nice.  

Now add `http.contains github` to the filter bar and open `http://jackar.github.io` on your mobile device. Voila, you should see a GET request in your feed. Double-click to view the packet and verify the host `jackar.github.io` and make sure to note the source IP.

#Step 3: Analyze Network Traffic
Now you can use the filter bar to analyze network traffic from your mobile device's local IP. If you want to analyze http traffic use `http && ip.addr == <your local ip here>`.