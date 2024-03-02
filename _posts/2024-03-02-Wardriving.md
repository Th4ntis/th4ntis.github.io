---
layout: post
title: "Wardriving"
categories: guide
author:
- th4ntis
---

War driving, also known as "WiFi sniffing" is the process of locating WiFi networks, and potentially viewing their traffic. You can find more notes on wardriving [here on my gitbook](https://cybersec.th4ntis.com/networking/wireless/wardriving-wifi-sniffing).


The following sections will cover how to begin wardriving using an android device or a laptop that runs on Debian based linux distributions.
# Android
This part is very simple, and it doesn't take much at all. As long as you have a phone running android, you can install the [Wigle WiFi Wardriving app](https://play.google.com/store/apps/details?id=net.wigle.wigleandroid). With this you can make an account on [Wigle.net](https://wigle.net/) and keep track of your individual stats and uploads. Though that isn't required, you can upload anonymously as well.

## Starting
With the app open you can tap the `X` icon in the top right, and it will begin scanning and collecting the networks and bluetooth around you.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/d760f6fc-f4b6-4a90-949d-9bd82124b76b)

Let this run as long as you desire, the longer it runs and the more it goes, the more you collect. When we're done we can tap the WiFi Scanning looking symbol in the top right to stop scanning.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/a4458d9d-9c0f-4d18-a6c8-23979192b8ef)

When you're ready to upload it all for processing, just tap the button that says `UPLOAD TO WIGLE.NET` in the top left. After that click the 3 lines in the top left and select uploads.
Here you will see a timestamp of the year/month/day it was uploading and the status of it.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/d42384bd-a1f7-472e-b23a-7b61034714a4)

From there it's simple. You've done it, you've done some wardriving. Look around the app more, look at stats, rankings, map, news, etc.

# Linux
For this you will need a couple things, like WiFi Adapters, and the [Kismet](https://www.kismetwireless.net/) software. A GPS Adapter and the [GPSD](https://gpsd.io/) software is not *REQUIRED* but will make it a lot more fun and adds the ability to see where you seen a specific WiFi. 

## Installing Kismet
I am running Debian but this also work on other linux distros. I will be covering how I did this on Debian and other Debian based distros but they have [documentation](https://www.kismetwireless.net/docs/readme/installing/linux/#installing-from-source) on installing it from source.

As I am running Debian 12(Bookworm), we will run the following commands to install kismet

```
wget -O - https://www.kismetwireless.net/repos/kismet-release.gpg.key --quiet | gpg --dearmor | sudo tee /usr/share/keyrings/kismet-archive-keyring.gpg >/dev/null

echo 'deb [signed-by=/usr/share/keyrings/kismet-archive-keyring.gpg] https://www.kismetwireless.net/repos/apt/release/bookworm bookworm main' | sudo tee /etc/apt/sources.list.d/kismet.list >/dev/null

sudo apt update && sudo apt install -y kismet gpsd gpsd-clients gpsd-tools
```

From here we need any WiFi radio we can use, including the built in one. I myself have and recommend these adapters:

* [Alfa AWUS036ACM](https://www.amazon.com/Alfa-AWUS036ACM-Long-Range-Dual-Band-Wireless/dp/B073X6RL9D) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036ACHM](https://www.amazon.com/gp/product/B08SJBV1N3/ref=ox\_sc\_act\_title\_1?smid=A20G3A026MV70R\&psc=1) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036ACH](https://www.amazon.com/dp/B08SJC78FH?ref\_=cm\_sw\_r\_cp\_ud\_dp\_PSZZG6J9X0XH40GXB685) <-- Capable of 2.4GHz and 5GHz (This more than likely \*will\* require driver installation)
* [Panda Wireless PAU09 N600](https://www.amazon.com/Panda-Wireless-PAU09-Adapter-Antennas/dp/B01LY35HGO) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036NEH](https://www.amazon.com/AWUS036NEH-Range-WIRELESS-802-11b-USBAdapter/dp/B0035OCVO6)
* [Alfa AWUS036NH](https://www.amazon.com/Alfa-AWUS036NH-802-11g-Wireless-Long-Range/dp/B003YIFHJY)
* [Panda Wireless PAU05](https://www.amazon.com/Panda-300Mbps-Wireless-USB-Adapter/dp/B00EQT0YK2)
* [TP-Link TL-WN722N](https://www.amazon.com/TP-Link-TL-WN722N-Wireless-network-Adapter/dp/B002SZEOLG)
* [Ralink USB WiFi RT5370](https://www.amazon.com/Ralink-RT5370-Raspberry-Adapter-Function/dp/B019XUDHFC)

If you're looking for GPS adapters, I also have the following, I primarily use the [GlobalSat BU-353-S4](https://www.amazon.com/GlobalSat-BU-353-S4-Receiver-Black-Improved-New/dp/B098L799NH/ref=sr_1_1?crid=2WAQ665IR5UV1\&keywords=GlobalSat+BU-353-S4\&qid=1660969339\&s=electronics\&sprefix=globalsat+bu-353-s4+%2Celectronics%2C148\&sr=1-1)
* [VK-162](https://www.amazon.com/dp/B01EROIUEW?ref=ppx_pop_mob_ap_share)
* [HiLetgo VK172](https://www.amazon.com/dp/B01MTU9KTF?ref=ppx_pop_mob_ap_share)

# Setting up Kismet
Before we start, we should make a couple quick modifications. We can run kismet as normal and collect multiple things but if we're going to be wardriving, we want to either add `--override wardrive` to the end of the command... OR we can add the wardrive settings to our `/etc/kismet/kismet_site.conf` file. This file will be our config and won't be modified any time kismet updates.

In our `kismet_site.conf` file we want to add a source, as in a WiFi or bluetooth device, our GPS configuration if we are using a GPS dongle/adapter, and our wardriving settings.

I run with multiple adapters and a [WiFi Coconut](https://shop.hak5.org/products/wifi-coconut). We need to find the name of our sources first. To find these, and grab the name of it run:
```
ip a
```

![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/c642311e-a694-4102-9dcb-c6c121b1e475)

We want to add that to our `kismet_site.conf` file.

![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/574c5734-f099-4902-8521-4cbba0aad9b1)

## GPS
If we have a GPS adapter and want to use that, we need to tell the system to use that, depending on your device it will be either `dev/ttyUSB0` or `/dev/ttyASM0` then set it with with `gpsd`. 

The easiest way to find this is to run the following before and after plugging in your adapter:
```
ls /dev/tty*
```


![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/422e6ff2-2eef-48e5-8160-48b1582a4a58)

After we have that we have gpsd use that device with:
```
gpsd /dev/ttyUSB0
```
and verify it's working with
```
gpsmon
```
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/8dea8090-d307-40a9-a6c8-a3f62e778354)

OR
```
cgps
```
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/d75350e2-09fb-4563-900c-5957a6572235)

Once we have that set, we can add our GPS into our `kismet_site.conf` file as well.

Now last of all, is to add the wardriving mode to our .conf file as well. We don't HAVE to do this, we can simply just run
```
kismet --override wardrive
```
to run it in wardrive mode. But if we are going to be using it for wardriving by default, we can take the `/etc/kismet/kismet_wardrive.conf` and add that to our `kismet_site.conf` file. Either one is fine.

We can copy/paste the contents in there or run the following to automatically copy all the contents over:
```
cat /etc/kismet/kismet_wardrive.conf | sudo tee -a /etc/kismet/kismet_site.conf > /dev/null
```


So now our `/etc/kismet/kismet_site.conf` file should look like this
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/e4c101fd-d79f-4ace-9d40-58c14bbef138)

With everything plugged in and on, we can now run kismet!

# Running Kismet
Simply run
```
kismet
````

or if you didn't add the wardrive.conf to your site.conf, run:
```
kismet --override wardrive
``` 
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/576d67cf-75f9-47a4-bbe2-4ee165ea32fb)

Then we're wardriving! We can verify our everything by either watching the screen fill with info
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/1eb6445f-ffe5-484e-8c4e-059ca45827c7)

We can also go to the kismet GUI with http://localhost:2501

When first goin here it will ask you to set a username and password. But when we're in, if you have GPS enabled you will see your coordinates in the top right.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/58abbd2f-bba0-4305-94fa-9f7a80da4641)

We can also see the WiFi sources we have enabled by going to to the 3 linesi nthe top left, and selecting `Data Sources`.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/30742b6a-3ab9-4203-8280-ba80051c5b61)

You're up and wardriving!

# Post Capture
When you've finished your drive, youll have some files.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/973d96c2-eb44-42a4-ab03-4addd636c8a0)

You can take the .wiglecsv file, upload it into [Wigle.net](https://wigle.net/) to your stats.

If we have GPS enabled and the info, we can convert the file into a KML File to be used with [Google Earth](https://earth.google.com/web/). [More info here](https://www.kismetwireless.net/docs/readme/kml/).

```
kismetdb_to_kml --in some-kismet-log-file.kismet --out some-kml-file.kml
```

We are also able to convert the .kismet file to a .pcap file to be analyzed in [Wireshark](https://cybersec.th4ntis.com/networking/wireshark). Documentation on that can be found [here](https://www.kismetwireless.net/docs/readme/kismetdb_to_pcap/).

```
kismetdb_to_pcap --in some-kismet-log.kismet --out some-pcap-log.pcapng
```

# Conclusion
That's how you wardrive!

Keep in mind when running this on a laptop, you'll want to make sure you have a long enough battery, the ability to charge it and your power settings are correct. I take mine, make sure it does nothing when the laptop lid is closed and close it that way I consume as little battery as possible.

This is also 100% doable with a smaller machine, like a [RaspberryPi](https://www.raspberrypi.com/) or a [Zimaboard](https://www.zimaboard.com/), which are smaller and much more portable, but you still need to keep an eye on the battery, as well as have some extra steps in there such as auto launching, the ability to SSH/VNC into it while out if needed/wanted. There will be a guide on that coming soon to be on the lookout!
