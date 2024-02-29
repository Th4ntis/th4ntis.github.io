---
layout: post
title: "Setting Up a Windows Server 2022 VM"
categories: guide
author:
- th4ntis
---

The same startup we did in the [Windows Server 2019 Setup](https://blog.th4ntis.com/Windows-Server-2019-Setup). After we start our VM and be ready to push a button to boot to the ISO as the screen will say. This is a pretty standard install, mostly just clicking next.

Once we get to this screen, we have some options. Typical it's with or without Desktop experience. I'm use to Desktop Experience with 2019 and previous, so i'm going to switch it up this time go with  out the Desktop experience so it's just CLI(Command Line Interface).

Select the drive and click next and let it install, it will automatically reboot. Once it reboots, follow the prompts, set your admin password. and you'll be brought to the SConfig menu.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(332).png)

I would start by going to 6, then 1 to update everything

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(274).png)

Once updated are complete and a reboot is done, I recommend installing VMWare tools. First we need to shutdown the VM and remove the ISO file. Edit the VM Settings. I'm choosing physical drive simply because my machine does not have a CD Drive. After you've powered back on the machine, from the VMWare menu > VM > Install VMWareTools. Then from the SConfig, type 15 to go to the powershell menu, go to the D: Drive and running setup64.exe

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(320).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(652).png)

We may need to move the window to see the VMWare Tools window.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(395).png)

After this install and reboots, we can shut it down.

Now is when you will want to shut down the VM and adjust RAM and Processors if needed. Drop it for 2GB or 4GB of RAM and 1 or 2 processors. I **HIGHLY** recommend creating a snapshot after you have this done and setup so that way you can always revert back to that snapshot if needed if something breaks or you just need to clean things up.

Now I am going to take this tip from John Hammond. After you create the Snap Shot, I recommend going into the VMS options, changing the name to Some form of Template and Options and enabling Template mode. So clone the VM choosing the Snapshot when we want to make a VM using this one so we don't have to re-create the VM from scratch every time.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(706).png)

# Domain Controller Setup

We should first run `SConfig` to change the hostname and set a static IP. After this we can run the following command to turn this into a DC.

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Import-Module ADDSDeployment
install-ADDSForest
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(463).png)

Now once it has been restarted we can add computers to the domain. We can do this via GUI, or command line with

`Add-Computer -DomainName web.local -Credential web\Administrator -Force -Restart` - Changing the web.local and web\Administrator, to what you changed your DomainName to from the previous step.
