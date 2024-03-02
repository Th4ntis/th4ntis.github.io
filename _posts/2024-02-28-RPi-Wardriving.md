---
layout: post
title: "Wardriving with a Raspberry Pi"
categories: guide
author:
- th4ntis
---

Basic info of War driving can be found [here](https://cybersec.th4ntis.com/networking/wireless/wardriving-wifi-sniffing). War driving, also known as "WiFi sniffing" is the process of locating WiFi networks, and potentially sniffing their traffic. I am running this on a RaspberryPi 4 but you can run this on any linux host, though for wardriving you want it as portable as possible.

# Hardware
* I've found running on [RaspberryPi](https://www.raspberrypi.com/) (I use a 3B+ and a 4) running [Ubuntu](https://ubuntu.com/download/raspberry-pi) is easiest.

I have and recommend the following adapters:

## WiFi Adapters

* [Alfa AWUS036ACM](https://www.amazon.com/Alfa-AWUS036ACM-Long-Range-Dual-Band-Wireless/dp/B073X6RL9D) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036ACHM](https://www.amazon.com/gp/product/B08SJBV1N3/ref=ox\sc_act_title_1?smid=A20G3A026MV70R\&psc=1) <-- Capable of 2.4GHz and 5GHz
* [Alfa AWUS036ACH](https://www.amazon.com/dp/B08SJC78FH?ref_=cm_sw_r_cp_ud_dp_PSZZG6J9X0XH40GXB685) <-- Capable of 2.4GHz and 5GHz (This more than likely \*will\* require driver installation)
* [WiFi Coconut](https://shop.hak5.org/collections/wifi-pentesting/products/wifi-coconut) <-- Capable of only 2.4GHz BUT has 14 integrated WiFi radios so you can be on all channels, all the time.

## GPS Adapters

* [GlobalSat BU-353-S4](https://www.amazon.com/GlobalSat-BU-353-S4-Receiver-Black-Improved-New/dp/B098L799NH/ref=sr_1_1?crid=2WAQ665IR5UV1\&keywords=GlobalSat+BU-353-S4\&qid=1660969339\&s=electronics\&sprefix=globalsat+bu-353-s4+%2Celectronics%2C148\&sr=1-1)
* [VK-162](https://www.amazon.com/dp/B01EROIUEW?ref=ppx_pop_mob_ap_share)
* [HiLetgo VK172](https://www.amazon.com/dp/B01MTU9KTF?ref=ppx_pop_mob_ap_share)

# Software

* [Kismet](https://www.kismetwireless.net/) - A powerful and popular tool made by [Dragorn](https://twitter.com/KismetWireless). "Kismet is a wireless network and device detector, sniffer, wardriving tool, and WIDS (wireless intrusion detection) framework.It works with Wi-Fi interfaces, Bluetooth interfaces, some SDR (software defined radio) hardware like the RTLSDR, and other specialized capture hardware."
  * To install kismet, follow [the guide on their docs](https://www.kismetwireless.net/docs/readme/packages/).
  * Kismet Config files readme can be found [here](https://www.kismetwireless.net/docs/readme/configuring/configfiles/).
  * Kismet wardriving overlay docs can be found [here](https://www.kismetwireless.net/docs/readme/configuring/wardrive/).&#x20;
* [GPSD](https://en.wikipedia.org/wiki/Gpsd) - **gpsd** is a computer software program that collects data from a GPS receiver and provides the data via an IP network to potentially multiple client applications in a server-client application architecture.
  * This can be installed on most Ubuntu/Raspbian `sudo apt install gpsd gpsd-clients`

Raspbian Install (I usually go with Nightly builds):

```
wget -O - https://www.kismetwireless.net/repos/kismet-release.gpg.key | sudo apt-key add -
echo 'deb https://www.kismetwireless.net/repos/apt/git/buster buster main' | sudo tee /etc/apt/sources.list.d/kismet.list
sudo apt update
sudo usermod -aG kismet your-user-here
sudo apt install kismet
```

Ubuntu Install (I usually go with Nightly builds):

```
wget -O - https://www.kismetwireless.net/repos/kismet-release.gpg.key | sudo apt-key add -
echo 'deb https://www.kismetwireless.net/repos/apt/git/jammy jammy main' | sudo tee /etc/apt/sources.list.d/kismet.list
sudo apt update
sudo usermod -aG kismet your-user-here
sudo apt install kismet
```

##

***

# Setup

## kismet_site.conf file

As long as you have a large enough MicroSD the Pi is running on, you _**should**_ be fine BUT if you would like to use an external HDD or USB Drive for storage, we need to set that to automount.

Verify the drive you want mounted with: `df -h` andverify where it's mount location.

We need to find the UUID of the Drive we mounted, most likely will be `/dev/sda1` but not always the case, so be sure to verify. Find the UUID with: `sudo blkid /dev/sda1`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(576).png)

We need to create a directory for this to me auto mounted on boot: `sudo mkdir -p /mnt/usb1`

Now change the ower of the newly made directory: `sudo chown -R th4ntis:th4ntis /mnt/usb1`(changing th4ntis with your username)

To set this to auto mount on startup I edited my `/etc/fstab` with: `sudo nano /etc/fstab`

Then added this to the bottom of the file: `UUID=[UUID] /mnt/usb1 [TYPE] defaults,auto,users,rw,nofail,noatime 0 0` changing the \[UUID] and \[TYPE] to the UUID and type of your drive when we used `sudo blkid /dev/sda1`.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(321).png)

We have a couple options, we can edit the kismet.conf file OR use the kismet_site.conf file. I chose the kismet_site.conf file.

Edit the file: `sudo nano /etc/kismet/kismet_site.conf`

I changed the log location to the external HDD or USb Drive location, as well as added the wiglecsv format for logging to upload

```
# Changes log location
log_prefix=/dev/usb1

# Turn on wiglecsv format
log_types+=wiglecsv
```

I took the setup from the `kismet_wardrive.conf` and added it to the end my `kismet_site.conf`

```
# Kismet wardrive mode

# This is an example of an override config file.  Override configs can be
# selected with the '--override' option when launching Kismet, for example:
#
# kismet --override wardrive

# Override configurations can be combined with normal configuration files,
# as well as kismet_site.conf.  For more information, check the Kismet docs:
# https://www.kismetwireless.net/docs/readme/config_files/#configuration-override-flavors

# This configuration sets several options to optimize Kismet for wardriving.
# It will only track and log Wi-Fi access points (other phy types like rtl433 and 
# bluetooth are logged normally).  In general it configures Kismet to use less RAM
# and disk space whenever possible.


# Notify that we're in wardriving mode and will not be capturing full data
load_alert=WARDRIVING:Kismet is in survey/wardriving mode.  This turns off tracking non-AP devices and most packet logging.


# Turn on wiglecsv format
log_types+=wiglecsv


# Turn off HT20, HT40, and VHT options on wifi datasources (unless they explicitly set them)
dot11_datasource_opt=ht_channels,false
dot11_datasource_opt=vht_channels,false
dot11_datasource_opt=default_ht20,false
dot11_datasource_opt=expand_ht20,false
# Set to only 802.11 management and eapol frames on all datasources
dot11_datasource_opt=filter_mgmt,true

# Only track access points; this prevents Kismet from tracking non-AP Wi-Fi devices,
# such as clients, probing devices, wired devices visible from the Wi-Fi network, etc.
dot11_ap_only_survey=true

# No need to fingerprint devices
dot11_fingerprint_devices=false

# Don't keep IE tags in RAM
dot11_keep_ietags=false

# Don't keep eapol in RAM
dot11_keep_eapol=false


# Turn off logging we don't use in wardriving scenarios

# Don't log channel use
kis_log_channel_history=false

# Don't log datasource counts
kis_log_datasources=false
```

Now we need to add the WiFi Radios, GPS, and Bluetooth sources to the kismet_site.conf

After plugging in your WiFi Radios, GPS, and Bluetooth adapters, depending which one you have, you'll wanna set the GPSD to the proper adapter. We can run `dmesg` to find the location of the USB device. The usual locations are:

## WiFi Radios:

Let's get the radio 'names' with: `ip a`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(265).png)

As I am using a WiFi Coconut, I will be having a lot of WiFi Radios. So take the WiFi interface name, eg. wlx0cefafd1408b, and copy as many of them as you have/will be using. Then we will edit the `kismet_site.conf` file and add the sources to that.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(159).png)

## GPS:

GlobatSat BU-353-S4:

```bash
/dev/ttyUSB0
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(638).png)

VK-162/VK172:

```
/dev/ttyASM0
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(325).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(653).png)

With the device plugged in, set GPSD to the device, it shouldn't return an error, if it does you may need to troubleshoot the error.

```
gpsd /dev/ttyUSB0
```

OR

```
gpsd -b /dev/ttyUSB0
```

To verify if it is working properly we can run `gpsmon` **OR** `cgps`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(643).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(626).png)

Now, in our `kismet_site.conf`, we will add GPSD as a GPS source

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(297).png)

***

# Running - Normal Mode

Now we can start and run kismet! We need to specify the WiFi Adapter and gps.

```
kismet -c (interface) gps=gpsd:host=localhost,port=2947,reconnect=true
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(487).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(261).png)

Now as the banner at the top says, we can go to the web interface at [http://localhost:2501/](http://localhost:2501/).

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(430).png)

If you don't specify an interface in the original command, when on the dashboard, you can select the 3 Lines in the top left, select 'Datasources' and enable the sources you want to use.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(593).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(531).png)

From here we can verify the GPS is working with the green cross hair icon in the top right, as well as seeing the info.

***

# Running - Wardrive Mode

If you're on the newest kismet version (2022-01-git and subsequent releases) we can run kismet in a specified [wardriving mode](https://www.kismetwireless.net/docs/readme/wardriving/).

```
kismet -t some_wardrive --override wardrive
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(471).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(516).png)

and just as above, If you don't specify an interface in the original command, when on the dashboard, you can select the 3 Lines in the top left, select 'Datasources' and enable the sources you want to use.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(593).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(531).png)

From here we can verify the GPS is working with the green cross hair icon in the top right, as well as seeing the info.

***

## Autostarting Kismet

The README for starting Kismet at launch can be found [here on their github](https://github.com/kismetwireless/kismet/blob/master/packaging/systemd/README).

As I installed Kismet from the package, the service for systemd is already there.

```
By default, the Kismet systemd service runs Kismet as root; this is NOT best practices
    but it is the only user consistently available.

    It is STONGLY recommended that you install Kismet as suid-root via `make suidinstall`,
    and that you run Kismet as a non-privileged user.  Kismet will then limit root 
    access to the capture binaries which control individual interfaces.
```

So lets set this up to run as our user. `sudo systemctl edit kismet` so edit the service. Changing the user to the 'kismet' user **OR** as the user you have setup.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(285).png)

So with this setup, let's start the service with `sudo service kismet start`.

Set the service to start on boot with: `sudo systemctl enable kismet`.

Verify the Kismet service is running with: `sudo service kismet status`.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(490).png)

***

# Post Capture

## Normal Mode

This will automatically log all traffic to a Kismet log file with the date from the directory where the command was run.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(423).png)

If we have GPS enabled and the info, we can convert the file into a KML File to be used with [Google Earth](https://earth.google.com/web/). [More info here](https://www.kismetwireless.net/docs/readme/kml/).

```bash
kismetdb_to_kml --in some-kismet-log-file.kismet --out some-kml-file.kml
```

We are able to convert the file to pcap to be analyzed in [Wireshark](../networking/wireshark.md). Docs can be found [here](https://www.kismetwireless.net/docs/readme/kismetdb_to_pcap/).

```
kismetdb_to_pcap --in some-kismet-log.kismet --out some-pcap-log.pcapng
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(205).png)

We can also upload the logs to [Wigle.net](https://wigle.net/index). Docs can be found [here](https://www.kismetwireless.net/docs/readme/wigle/).

```
kismetdb_to_wiglecsv --in some-kismet-log-file.kismet --out some-wigle-file.csv
```

You can then upload it.

## Wardrive Mode

This mode will automatically create 2 files: a kismet file and a wiglecsv file to upload to [Wigle.net](https://wigle.net/index). Docs can be found [here](https://www.kismetwireless.net/docs/readme/wardriving/). This will sho that logging is greately reduced and will only be used  for Access Point(AP) collection.

```
kismet --override wardrive
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(362).png)

If we have GPS enabled and the info, we can convert the file into a KML File to be used with [Google Earth](https://earth.google.com/web/). [More info here](https://www.kismetwireless.net/docs/readme/kml/).

```bash
kismetdb_to_kml --in some-kismet-log-file.kismet --out some-kml-file.kml
```

We are able to convert the file to pcap to be analyzed in [Wireshark](../networking/wireshark.md). Docs can be found [here](https://www.kismetwireless.net/docs/readme/kismetdb_to_pcap/).

```
kismetdb_to_pcap --in some-kismet-log.kismet --out some-pcap-log.pcapng
```
