---
layout: post
title: "Setting Up a Windows 11 VM"
categories: guide
author:
- th4ntis
---

A virtual machine(VM) is the virtualization or emulation of a computer system. It's a way to run a computer system such as windows or linux. You can use this to test new/other features, software, configurations, etc.

VM software that is used to emulate other computer systems are [VMWare Workstation Player](https://www.vmware.com/products/workstation-player.html) (Free version), [VMWare Workstation Pro](https://www.vmware.com/products/workstation-pro.html) (Paid version), or [Virtualbox](https://www.virtualbox.org/). I have VMWare Workstation Pro but Player works just as well. You can go with either VMWare or Virtualbox, both work and it just comes down to personal preference on the application and what you're use to. There's minor differences between the two for most end users.

Obviously your setup may differ depending on your system specs. I typically go with 4GB(4096 MB) of RAM per VM, 2 processors and 2 cores per processor but I have a bit beefier machine. If you need to, you can start with 4 or 8GB of RAM and 2 processors and 2 core per processor, for the install so it goes faster then drop it down to 2 or 4GB of RAM and 2 processors and 1 core per processor.

We start the same way as [Windows 10 VM Setup](https://blog.th4ntis.com/Windows10-Machine-Setup) but once met with this screen telling us we don't meet the requirements...

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(548).png)

Press `Shift+F10` to bring up the Command Prompt

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(712).png)

Run:
```
regedit
```

and navigate to `HKEY_`_`LOCAL_MACHINE\SYSTEM\Setup` and make a new Key called:

```
LabConfig
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(745).png)

_Inside there create DWord(32-Bit) Values for:_

* _BypassTPMCheck_
* _BypassRAMCheck_
* _BypassSecureBootCheck_

_and change their value to 1_

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(713).png)

Close out that window to exit the installation and start from the beginning window.

Click INSTALL NOW, then accept the EULA and click next, now we continue as normal

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(283).png)

After install and it reboots

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(779).png)

We choose our region, keyboard layout, etc. and we can setup our account. Select 'sign-in options'

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(242).png)

Then 'Domain Join Instead'

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(390).png)

Input our username and password, password confirmation, and security questions

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(462).png)

Disable all the privacy settings and click accept.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(290).png)

It will now do Windows setup and such

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(768).png)

We're now logged in and can install VMWare Tools

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(346).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(454).png)

***

# Joining a domain

Same way we would for [Windows 10 VM Setup](https://blog.th4ntis.com/Windows10-Machine-Setup). Open the start menu and search for domain, and select 'Access work or school'

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(359).png)

Click the blue 'Connect' button

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(546).png)

Select 'Join this device to a local Active Directory domain'.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(198).png)

and follow the steps. Add in your domain name, sign in with Admin credentials, reboot, and ta-da!
