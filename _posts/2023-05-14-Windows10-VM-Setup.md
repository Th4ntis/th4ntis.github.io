---
layout: post
title: "Setting Up a Windows 10 VM"
categories: guide
author:
- th4ntis
---

A virtual machine(VM) is the virtualization or emulation of a computer system. It's a way to run a computer system such as windows or linux. You can use this to test new/other features, software, configurations, etc.

VM software that is used to emulate other computer systems are [VMWare Workstation Player](https://www.vmware.com/products/workstation-player.html) (Free version), [VMWare Workstation Pro](https://www.vmware.com/products/workstation-pro.html) (Paid version), or [Virtualbox](https://www.virtualbox.org/). I have VMWare Workstation Pro but Player works just as well. You can go with either VMWare or Virtualbox, both work and it just comes down to personal preference on the application and what you're use to. There's minor differences between the two for most end users.

Obviously your setup may differ depending on your system specs. I typically go with 4GB(4096 MB) of RAM per VM, 2 processors and 2 cores per processor but I have a bit beefier machine. If you need to, you can start with 4 or 8GB of RAM and 2 processors and 2 core per processor, for the install so it goes faster then drop it down to 2 or 4GB of RAM and 2 processors and 1 core per processor.

# Windows install

You can find a link to Evaluation ISOs [here](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise) on Microsofts website.

## Starting

We will do a typical install beginning

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(520).png)

Choose to install the System later

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(604).png)

Select the Windows OS.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(287).png)

Select a path and name the VM. For this I will be naming the VM after the User that I plan on using on this VM.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(253).png)

Select the size of the VM. This will **NOT** the overall size, this is just the max size of the VMs HDD space and will fill up as we add more to the VM that takes up space.

If this VM will be on a PC and not be used from an external HDD or moved around you can store it as a single file but if you plan on using this VM on other PC or from an external HDD it's a better idea to split it into multiple files.

Depending on your space, you can edit how much you want. I usually go the default 60GB.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(447).png)

Finally we can now customize our hardware. This is where we can customize the RAM, Processors, ISO files, Network Settings, etc. This is where we select our Windows Server ISO.

I recommend disabling the Printer, Sound Card, and under Display unchecking 'Accelerate 3D Graphics'.

I usually increase the RAM for the install so it goes quicker then drop it down after.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(397).png)

Start the VM and press any button to boot from the ISO.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(668).png)

Click INSTALL NOW, then accept the EULA and click next

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(283).png)

Custom Install

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(541).png)

Click Next

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(315).png)

Let it install

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(426).png)

After install and it reboots

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(738).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(496).png)

Skip adding a second layout, unless you would like one

Click Domain Join instead when asked to Sign Into Microsoft

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(511).png)

Enter the Username and Password. I recommend a basic password (Eg. Password1, Password 123, etc.) for the user(s) since this for testing purposes. Then for Security Questions, you can put whatever you want, I wouldn't use real world info as this is for a lab.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(472).png)

Turn off all settings

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(667).png)

Choose "Not now"

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(441).png)

Let the install finish with post setup stuff

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(188).png)

# Post Install

Now we have our desktop. Time to do some basic setup.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(523).png)

Install VMWare Tools by:
```
In the Menu Bar of VMWare > VM > Install VMWare Tools
```

Inside the VM:
```
open File Explorer > This PC > Run the VMWare Tools installer
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(453).png)

Then a basic click next on everything. Don't reboot when done since you'll want to reboot after you rename the PC as well.

Open Start Menu, type in `Rename`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(167).png)

Rename this PC

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(666).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(633).png)

Reboot to apply changes and you're done! BUT if you want to add it to a domain, we continue on.

Now is when you will want to shut down the VM and adjust RAM and Processors if needed. Drop it for 2GB or 4GB of RAM and 1 or 2 processors.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(201).png)

***

# Joining a Domain

If you're looking to add this machine to a domain to emulate a corporate environment, you'll want to make sure you setup a [Windows Server 2019 VM](https://th4ntis.com/guide/2023/05/16/Windows-Server-2019-Setup.html) or a [Windows Server 2022 VM](https://th4ntis.com/guide/2023/05/17/Windows-Server-2022-Setup.html) first.

You _may_ want a second user machine as well but only if you're machine is capable of it. It's not required but it will help. This process will be the same for both machines if setting up two.

First we need to get the IP of our Domain Controller by opening Command Prompt and running
```
ipconfig
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(507).png)

We need to set the users machine DNS to our Domain Controllers IP.

On the Users machine: 

```
Start Menu > Setting Icon > Network & Internet > Change Adapter Options
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(280).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(714).png)

Right click on the Ethernet Adapter

```
Properties > Double Click on Internet Protocol Version 4 (TCP/IPv4)
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(458).png)

Change the DNS option to your Domain Controllers IP > OK

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(273).png)

From here on the Users machine still: 

```
Start Menu > Domain > Access Work or School
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(778).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(534).png)

Click connect then at the bottom of the window select "Join a local Active Directory Domain"

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(783).png)

It'll ask for the domain name

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(246).png)

Then should ask who do you want to join as. At first join as Admin.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(445).png)

Skip this step

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(310).png)

Then Reboot. Once it's rebooted and at the login screen, select "Other User" In the bottom left.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(465).png)

Input the username and password of a user you created

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(642).png)

After you have signed in with a user you created. Sing out and sign back in as the Administrator. You will need to add the domain before the Administrator name or it will try to log you in as the local admin.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(598).png)

We're going to add the user we logged in as, as an admin on this computer

```
Start Menu > Computer Management > Local Users and Groups > Groups > Administrators
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(675).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(272).png)

Double Click on Administrators > Add > Type in user name > Check Names > Ok

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(278).png)

Apply > Ok

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(481).png)

All set! We now have a lab Environment with Ubuntu for an attack machine if needed, a Windows Domain Controller/Server and a User or Two. We can now install whatever tools or software we may want onto them. I **HIGHLY** recommend creating a snapshot after you have this done and setup so that way you can always revert back to that snapshot if needed if something breaks or you just need to clean things up.

