---
layout: post
title: "WiFi Pineapple Basics"
categories: guide
author:
- th4ntis
---

The [WiFi Pineapple](https://shop.hak5.org/products/wifi-pineapple) from [Hak5](https://hak5.org/) is a wireless auditing platform that allows network security administrators to conduct wireless penetration tests. It has multiple features including the ability to create rogue access points, man-in-the-middle attacks, perform passive surveillance, WPA and WPA Enterprise attacks, and more. More info can be found on [their docs page](https://docs.hak5.org/wifi-pineapple).

By default it uses 2.4GHz frequency but you can get their `MK7AC WiFi Adapter` that will add 2.5GHz and 5GHz frequencies. You are able to use your own adapters but they not work well with the pineapple, so your milage may vary depending on device.

# Setup
## Windows
Now if we want to share our internet connection from our PC to the Pineapple, we need to also select the interface that our internet comes in on(Wi-Fi in my case) and share it to the Pineapple.
![Interfaces](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/852aa8cb-9932-49d6-b2b1-6e7918f73bf6)
![Internet Sharing](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/4e8620c8-c327-4a9a-8e2b-161a71bd5910)

Now we need to set the IP for it:
```
IP: 172.16.42.42
Subnet: 255.255.0.0
Gateway: blank
DNS: Whatever you want, I usually use Cloudflare(1.1.1.1) or Quad9(9.9.9.9)
```
![IPv4 Properties](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/b93aff5f-5b8c-4967-a5d5-915b3e485fca)

## Linux
To share our internet from our host machine to the Pineapple, we can run a python script I made. The github page can found [here](https://github.com/Th4ntis/Debian-Internet-Sharing).

The script is
```python
#!/usr/bin/env python3
import subprocess

def run_command(command):
    """Run a shell command."""
    try:
        subprocess.check_call(command, shell=True)
    except subprocess.CalledProcessError as e:
        print(f"An error occurred: {e}")

def setup_internet_sharing(internet_interface, device_interface):
    """Set up internet sharing from the internet interface to another USB/RNDIS Device."""
    # Enable IP forwarding
    run_command("echo 1 > /proc/sys/net/ipv4/ip_forward")

    # Configure NAT over iptables
    run_command(f"iptables -t nat -A POSTROUTING -o {internet_interface} -j MASQUERADE")
    run_command(f"iptables -A FORWARD -i {internet_interface} -o {device_interface} -m state --state RELATED,ESTABLISHED -j ACCEPT")
    run_command(f"iptables -A FORWARD -i {device_interface} -o {internet_interface} -j ACCEPT")

    # Assign an IP address to the Devices interface if needed
    run_command(f"ifconfig {device_interface} 172.16.42.1 netmask 255.255.0.0")

    print("Internet sharing setup complete.")

if __name__ == "__main__":
    internet_interface = input("Enter the name of your internet-facing interface (e.g., eth0, wlan0): ")
    device_interface = input("Enter the name of your USB/RNDIS devices interface (e.g., eth1): ")
    setup_internet_sharing(internet_interface, device_interface)
```

This will ask for your internet-facing device, then ask for the Pineapples interface. We can find both of those with
```bash
ip a
```
![ip a](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/67749eb6-3366-4eb8-9b1c-3c0c4e2fb028)

then run the script
![script](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/f9fe58ab-1081-41f7-9fbf-943fbbe94547)

# WebUI
Now we can go to the Web Interface in our browser by going to http://172.16.42.1:1471/
![Web-UI-Welcome](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/0320d5f2-67fa-41bd-b7ba-27341f1b6369)

This option is up to you on which you prefer - I went with Radios disabled
![Verify Device](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/9082ce7c-f7d1-46bc-ac45-ae1b2d47d2b9)

After a few more prompts we will be asked for put our root password and timezone.
![Password Setup](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/7466d21f-13a3-40fa-ba00-0d1f814f5c39)


Now we set our AP's. The Management AP we will connect to to access the dashboard and tools and etc. The Open AP is the one your target(s) will be connecting to. I usually hide the management AP but it's up to you on what you would like to do.
![Network Setup](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/5588977a-7419-4474-9174-eee7c7452d98)

Now we get to an important step, the Client FIlter and SSID Filter. by default I put mine to allow that way not just anyone can connect and use it, we want our specified target(s) to connect. If we have their MAC address or SSID, we can add them now, or move onto the next step.
![Client Filter](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/6f6904fc-b150-446f-96d2-35b87ab935c2)
![SSID Filter](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/3b055003-af27-42cf-b982-6f620ebe8806)

Now choose your theme and accept their EULA. After a min or so we are redirected to the login page and after we login, we have the darhboard!
![Login Page](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/43214e2f-6737-4951-b848-1f1b2f395ecd)

If we have the MK7AC Adapter plugged in, we get a prompt for it, but we can close that for now.
![MK7AC Adapter](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/34f71fff-1709-451d-826b-576786b16811)

## Internet Setup
So we have some options for setting up the internet to the Pineapple. This step is important because it's what will keep traffic flowing throw the target(s) to the internet for them to stay connected to us.
- Wireless Client Mode - This will require us to have an SSID and Password for the Pineapple to connect to.
- ICS(Internet Connection Sharing) - This will allow us to share the internet connection from our machine the Pineapple is plugged into.
- USB Ethernet Adapter - If we have a USB to Ethernet dongle plugged into the Pineapple we can use this method for a hardwired connection.

When you know which one you want, select the `Network Setting` button. I typically prefer to go with ICS as we set that up before this.
![Internet Connection](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/3db54f61-27ef-43f9-8490-c9bc8956efdf)

While here, if we have the MK7AC module plugged in, we can change our `Recon Wireless Interface` to that and select save.
![Networking](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/6071c948-8cf3-4968-9218-a5851df8f2c3)

Whichever option you choose, you can verify internet connection by clicking the terminal icon in the top right and ping something to test.
![Test COnnection](https://github.com/Th4ntis/th4ntis.github.io/assets/53808039/be8312bd-76b4-44c1-a80e-d3e0a8398208)
