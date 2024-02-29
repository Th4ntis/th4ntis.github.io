---
layout: post
title: "Setting up a Kali VM"
categories: guide
author:
- th4ntis
---

I typically prefer to install it myself([using the ISO](https://www.kali.org/get-kali/#kali-installer-images)), BUT you can grab a [pre-made VM](https://www.kali.org/get-kali/#kali-virtual-machines) (using [7-Zip](https://www.7-zip.org/download.html) to extract it) from them. I have VMWare Workstation Pro but Player works just as well. You can go with either VMWare or Virtualbox, both work and it just comes down to personal preference on the application and what you're use to.

Obviously your setup may differ depending on your system specs. I typically go with 4GB(4096 MB) of RAM per VM, 2 processors and 2 cores per processor but I am running with 32GB of RAM and an Intel i7-10750H.

If you need to, you can start with 4 or 8GB of RAM and 2 processors and 2 core per processor, for the install so it goes faster then drop it down to 2 or 4GB of RAM and 2 processors and 1 core per processor for the victim machines.

## Kali ISO

We will start with the Typical configuration, similar to what we did at the start of the [Ubuntu VM Setup](https://blog.th4ntis.com/Ubuntu-VM-Setup)

Now we close that and can start our VM and install it. Once we start the VM, it'll take us to a list, I'm going to choose Graphical Install

![](https://github.com/Th4ntis/CyberSecNotes/blob/main/.gitbook/assets/image%20(694).png)

Choose your language and Region

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(341).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(521).png)

Choose your Keyboard layout

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(294).png)

Give the machine a hostname

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(328).png)

If you have a domain name you would like this to utilize, input it here. I will be leaving mine blank.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(323).png)

Choose a username for your account

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(772).png)

Input a password for the user

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(726).png)

Choose the timezone for the VM

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(213).png)

For a VM, we will use the entire disk.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(176).png)

Pick the disk you would like to use

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(688).png)

Pick how you may want to partition the drive. Usually for a VM instance, you can put everything into one partition.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(164).png)

Verify the partition changes

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(751).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(505).png)

Let the VM Install

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(322).png)

Choose if you want another Desktop Environment(DE) or if you do/don't any extra tools to bein installed

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(377).png)

After that finishes installing, choose to install the GRUB bootloader

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(396).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(208).png)

Once install is finished, you'll beboot the system and done!

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(508).png)

***

# Kali VM

## VMWare / Virtualbox

The VM version is essentially the same just minorly different when choosing the file to open. VMWare looks for a `.vmx` file, while VirtualBox looks for a `.vbox` file. Upon launching, the default credentials are be the word `kali` for the username AND password.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(517).png)

Download the .7z file and extract it with [7Zip](https://www.7-zip.org/) in whatever manor that works for you.

### VMWare

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(741).png)

With the VM extracted, within VMWare, File > Open, then navigate to the directory with the .vmx file.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(347).png)

From here, you can see the default settings on the site, but can edit them better suited to your machine.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(237).png)

### Virtualbox

With the VM extracted, select the add button, then navigate to the directory with the .vbox file.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(452).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(629).png)

From here, you can see the default settings on the site, but can edit them better suited to your machine.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(540).png)

***

# Post Install

Once we're in, it's a good idea to open the terminal, and we update everything with `sudo apt update && sudo apt upgrade -y`.

Now is when you will want to shut down the VM and adjust RAM and Processors if needed. Drop it for 2GB or 4GB of RAM and 1 or 2 processors.

From here you can explore and find things, change settings, and learn. I **HIGHLY** recommend creating a snapshot after you have this done and setup so that way you can always revert back to that snapshot if needed if something breaks or you just need to clean things up.

I recommend using some additional software such as [PimpMyKali from DeWalt](https://github.com/Dewalt-arch/pimpmykali/blob/master/README.md).
