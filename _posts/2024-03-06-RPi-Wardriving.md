---
layout: post
title: "Wardriving with a Raspberry Pi"
categories: guide
author:
- th4ntis
---

War driving, also known as "WiFi sniffing" is the process of locating WiFi networks, and potentially viewing their traffic. You can find more notes on wardriving [here on my gitbook](https://cybersec.th4ntis.com/networking/wireless/wardriving-wifi-sniffing).

On my [previous post](https://th4ntis.com/guide/2024/03/02/Wardriving.html) about wardriving, this works no problem but sometimes you want a smaller, easier device to carry that is more portable. In comes the [RaspberryPi](https://www.raspberrypi.com/)!

# RaspberryPi
I've found running on RaspberryPi (I use a RPi4) running [Raspbian-Lite](https://www.raspberrypi.com/software/operating-systems/#raspberry-pi-os-64-bit) is easiest as it's very portable, easy to throw into a backpack, and easy to setup with a USB Hub if needed. I also recommend a 64GB+ Micro SD card to have enough space. I run a 128GB but I grab my files from the Pi when every so often. For this you will need a couple things other than a RaspberryPi, like WiFi Adapters, and the [Kismet](https://www.kismetwireless.net/) software. A GPS Adapter and the [GPSD](https://gpsd.io/) software is not *REQUIRED* but will make it a lot more fun and adds the ability to see where you seen a specific WiFi. 

## WiFi Radios
From here we need any WiFi radio we can use, including the built in one. I myself have and recommend these adapters:

* [Alfa AWUS036ACM](https://www.amazon.com/Alfa-AWUS036ACM-Long-Range-Dual-Band-Wireless/dp/B073X6RL9D) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036ACHM](https://www.amazon.com/gp/product/B08SJBV1N3/ref=ox\_sc\_act\_title\_1?smid=A20G3A026MV70R\&psc=1) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036ACH](https://www.amazon.com/dp/B08SJC78FH?ref\_=cm\_sw\_r\_cp\_ud\_dp\_PSZZG6J9X0XH40GXB685) <-- Capable of 2.4GHz and 5GHz (This more than likely *will* require driver installation)
* [Panda Wireless PAU09 N600](https://www.amazon.com/Panda-Wireless-PAU09-Adapter-Antennas/dp/B01LY35HGO) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036NEH](https://www.amazon.com/AWUS036NEH-Range-WIRELESS-802-11b-USBAdapter/dp/B0035OCVO6)
* [Alfa AWUS036NH](https://www.amazon.com/Alfa-AWUS036NH-802-11g-Wireless-Long-Range/dp/B003YIFHJY)
* [Panda Wireless PAU05](https://www.amazon.com/Panda-300Mbps-Wireless-USB-Adapter/dp/B00EQT0YK2)
* [TP-Link TL-WN722N](https://www.amazon.com/TP-Link-TL-WN722N-Wireless-network-Adapter/dp/B002SZEOLG)
* [Ralink USB WiFi RT5370](https://www.amazon.com/Ralink-RT5370-Raspberry-Adapter-Function/dp/B019XUDHFC)

## GPS
If you're looking for GPS adapters, I also have the following, I primarily use the [GlobalSat BU-353-S4](https://www.amazon.com/GlobalSat-BU-353-S4-Receiver-Black-Improved-New/dp/B098L799NH/ref=sr_1_1?crid=2WAQ665IR5UV1\&keywords=GlobalSat+BU-353-S4\&qid=1660969339\&s=electronics\&sprefix=globalsat+bu-353-s4+%2Celectronics%2C148\&sr=1-1)
* [VK-162](https://www.amazon.com/dp/B01EROIUEW?ref=ppx_pop_mob_ap_share)
* [HiLetgo VK172](https://www.amazon.com/dp/B01MTU9KTF?ref=ppx_pop_mob_ap_share)

# Flashing
I am using the [RaspberryPi Imager](https://www.raspberrypi.com/software/) software to flash Raspbian onto my Pi.

Choose your device, OS, and SD card.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/a6e2a10b-abba-48b3-9d66-f747f4ad3c7b)


We probably should edit the settings for this to connect it to our WiFi so I am able to SSH into it right away.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/99a9d0c7-3ac5-4324-be56-1ac766e6d580)
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/a49711be-8afb-4530-a605-5c7d3fe9a3e3)

Then from here, we save the settings and select yes
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/9c79a475-28d4-47e0-83f4-e340df810f3c)

It will inform us that all the current data on the MicroSD will be erased and ask if we want to conitue, we say yes.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/4d69e571-9583-4e39-9a13-30b8046d11b2)

Once it's done, you can plug the SD card into the Pi, and turn it on. Give it a few min to do it's thing.
If you're plugging your Pi into a Monitor with mouse/keyboard, you're good to continue. If not, onto a mild pain, finding the IP address of the Pi. You can use whatever method you are comfortable with. USUALLY you can log into your Router and find IP addresses of devices on your network. You can nmap/zenmap scan your network, whatever works for you.

Once you have the IP, you can ssh into the Pi using ssh if you're on MacOS or Linux, on windows you'll use [Putty software](https://putty.org/). Use the username you setup in the setting of the Imager software. If you went with default, the default username is `pi` and the default password is `raspberry`.

MacOS/Linux:
```
ssh [username]@[ip]
```

Windows:
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/04b81224-c0cf-47a0-b1e0-242294837b05)

Once you're logged in a good idea is to update your system:
```bash
sudo apt update && sudo apt upgrade -y
```
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/8c6a6304-fb72-4a3c-a55c-f2c2ef78eeca)

# Kismet
As this is on a RPi running Raspbian(Currently Debian Bookworm), we will follow [the instructions](https://www.kismetwireless.net/packages/#debian-bookworm) on installing on that. Other install instructions from packages can be found [here](.kismetwireless.net/packages/), or installing from source can be found [here](https://www.kismetwireless.net/docs/readme/installing/linux/#installing-from-source).

```bash
wget -O - https://www.kismetwireless.net/repos/kismet-release.gpg.key --quiet | gpg --dearmor | sudo tee /usr/share/keyrings/kismet-archive-keyring.gpg >/dev/null

echo 'deb [signed-by=/usr/share/keyrings/kismet-archive-keyring.gpg] https://www.kismetwireless.net/repos/apt/release/bookworm bookworm main' | sudo tee /etc/apt/sources.list.d/kismet.list >/dev/null

sudo apt update && sudo apt install -y kismet gpsd gpsd-clients gpsd-tools
```

When asked if Kismet should be installed with suid-root helpers, select yes by pressing enter.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/61637d6d-48c5-4295-8dbb-0c53ec2c67fb)

Add your user to the kismet group with:
```bash
sudo usermod -aG kismet your-user-here
```

# Setup
First we need to run kistmet to set a default username/password. So run:
```
kismet
```

Then go to `https://[IP]:2501` to get to a web interface and set the username and password.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/b8820f31-7f4a-4fad-abb8-e2d9d5863048)

After you've done that, press `CTRL+C` to stop kismet at the moment.

## Kismet config
So by default, the kismet config file is under `/etc/kismet/kismet.conf` but that gets reset every time we update kismet, so we will put our sonfig in a file called `kismet_site.conf` under the same directory of `/etc/kismet/`. To make this easier, we can take the kismet-wardrive mode config and copy that into our `kismet_site.conf` file with:
```bash
sudo cp /etc/kismet/kismet_wardrive.conf /etc/kismet/kismet_site.conf
```

After plugging in your WiFi Radios, GPS, and Bluetooth adapters, depending which one you have, you'll wanna set the GPSD to the proper adapter.

### GPS:
After plugging in our GPS adapter, we can run:
```bash
dmesg
```

In here we look for our GPS dongle, it willl usually look like:
`GlobatSat BU-353-S4`:
```bash
/dev/ttyUSB0
```
![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(638).png)

`VK-162/VK172`:
```
/dev/ttyASM0
```
![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(325).png)

With the device plugged in, set GPSD to the device
```
gpsd /dev/ttyUSB0
```

**OR**

```
gpsd -b /dev/ttyACM0
```

To verify if it is working properly we can run:
```bash
gpsmon
```
![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(643).png)

**OR**
```bash
cgps
```
![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(626).png)

Now, in our `kismet_site.conf`, we will add GPSD as a GPS source to the top of the config with:
```bash
sudo nano /etc/kismet/kismet_site.conf
```

```
gps=gpsd:host=localhost,port=2947
```
![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(297).png)


### WiFi Radios:
Let's get the radio 'names' with:
```
ip a
```
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/08923a96-2978-495d-acb8-554fa36698ee)

With only one radio plugged in, it should be wlan1 as wlan0 is the built in WiFi. With the source, we add that to our `kismet_site.conf` with our GPS and Wardrive config
```
source=wlan1
```
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/851c3898-226c-417b-91b7-e483bd91f01c)

# Running
From here - we can run kismet with
```
kismet
```
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/5c03793e-d406-480d-8e1c-16917806e17d)

It should start and be on it's way with collecting WiFi. Now what if we don't want to tell it to start every time? We want it to auto launch kismet when it powers on and starts up.


# Autostarting Kismet
The README for starting Kismet at launch can be found [here on their github](https://github.com/kismetwireless/kismet/blob/master/packaging/systemd/README).

As I installed Kismet from the package, the service for systemd is already there.
```
By default, the Kismet systemd service runs Kismet as root; this is NOT best practices but it is the only user consistently available.

It is STONGLY recommended that you install Kismet as suid-root via `make suidinstall`, and that you run Kismet as a non-privileged user.  Kismet will then limit root access to the capture binaries which control individual interfaces.
```

So lets set this up to run as our user with:
```
sudo systemctl edit kismet
```

Changing the user to the 'kismet' user **OR** as the user you have setup.
```
[Service]
User=th4ntis
Group=kismet
StartLimitBurst=0
```
![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(285).png)

So with this setup, let's start the service with"
```
sudo service kismet start
```

Set the service to start on boot with:
```
sudo systemctl enable kismet
```

Verify the Kismet service is running with:
```
sudo service kismet status
```
![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(490).png)

It's all set and ready to autostart!

# Post Capture
A kismet file and a wiglecsv file will be made. The wiglecvs is to upload to [Wigle.net](https://wigle.net/index). Docs can be found [here](https://www.kismetwireless.net/docs/readme/wardriving/). This will show that logging is greately reduced and will only be used  for Access Point(AP) collection.
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/08ccba20-1fa9-4e7b-a325-66e6f889305e)

To copy these files off, we will need to run this from our host machine
```bash
scp [user]@[ip]:/home/[user]/[kismet-file] .
```
![image](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/1bae39fe-50f2-4bcd-bb21-ad87052dbec0)

This will need to be the exact file name. We can also use something like [WinSCP](https://winscp.net/eng/index.php) to copy the files to our machine using Windiws.

If we have GPS enabled and the info, we can convert the file into a KML File to be used with [Google Earth](https://earth.google.com/web/). [More info here](https://www.kismetwireless.net/docs/readme/kml/).

```bash
kismetdb_to_kml --in some-kismet-log-file.kismet --out some-kml-file.kml
```

We are able to convert the file to pcap to be analyzed in [Wireshark](../networking/wireshark.md). Docs can be found [here](https://www.kismetwireless.net/docs/readme/kismetdb_to_pcap/).

```
kismetdb_to_pcap --in some-kismet-log.kismet --out some-pcap-log.pcapng
```

# Conclusion
Ta-da! You're even more portable with your wardriving! Just get a powerful enough powerbank to run it on the go, plug it in, put it all in a backpack and you're off! Enjoy Wardriving and Capturing All The WiFi.

