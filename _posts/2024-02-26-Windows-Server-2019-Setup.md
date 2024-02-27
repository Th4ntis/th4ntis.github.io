---
layout: post
title: "Setting Up a Windows Server 2019 VM"
categories: guide
author:
- th4ntis
---

I usually go with a [Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019) VM with [VMWare](https://customerconnect.vmware.com/en/downloads/details?downloadGroup=WKST-PLAYER-1623-NEW\&productId=1039\&rPId=85399) or [Virtualbox](https://www.virtualbox.org/). I have VMWare Workstation Pro but Player works just as well. You can go with either VMWare or Virtualbox, both work and it just comes down to personal preference on the application and what you're use to.

Obviously your setup may differ depending on your system specs. I typically go with 4GB(4096 MB) of RAM per VM, 2 processors and 2 cores per processor but I am running with 32GB of RAM and an Intel i7-10750H.

If you need to, you can start with 4 or 8GB of RAM and 2 processors and 2 core per processor, for the install so it goes faster then drop it down to 2 or 4GB of RAM and 2 processors and 1 core per processor for the victim machines.

# Windows Server 2019

Start with our Typical Configuration that we did in the [Windows 10 VM Setup](https://blog.th4ntis.com/Windows10-Machine-Setup) with the defaults. Once we load the ISO, boot, and "Install Now", Choose the "Windows Server 20XX Standard Evaluation (Desktop Experience)"

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(557).png)

Accept the EULA and do a Custom Install

Click Next

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(701).png)

Let it install and reboot, after install is done and bring you to a screen to set your Admin password.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(577).png)

After that you should be good to log in!

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(762).png)

Now we install VMWare tools with VM > Tools > Install VMWare Tools

Now we can open File Explorer and go to "This PC" to run the Installer.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(775).png)

This is a typical install, just click next through it all. This will ask to restart the PC, but don't do that yet, we will have to restart anyway in the next parts of the setup.

Now is when you will want to shut down the VM and adjust RAM and Processors if needed. Drop it for 2GB or 4GB of RAM and 1 or 2 processors.

I **HIGHLY** recommend creating a snapshot after you have this done and setup so that way you can always revert back to that snapshot if needed if something breaks or you just need to clean things up.

Now I am going to take this tip from [John Hammond](https://twitter.com/_JohnHammond). After you create the Snap Shot, I recommend going into the VMS options, changing the name to Some form of Template and Options and enabling Template mode. So clone the VM choosing the Snapshot when we want to make a VM using this one so we don't have to re-create the VM from scratch every time.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(706).png)

# Domain Controller Setup

We have our Server Dashboard open

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(174).png)

To set this up for our domain controller, in top right: Manage > Add Roles and Features

Most of this will be standard of clicking next

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(528).png)

Role-based or feature-based Installation

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(648).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(764).png)

Now is where we select "Active Directory Domain Services", with the popup, Select Add Features, then click next.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(681).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(163).png)

From here it'll be "Next" till the end. Click Install and let it install

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(149).png)

Installed!

![](https://github.com/Th4ntis/CyberSecNotes/blob/main/.gitbook/assets/image%20(722).png)

Now we should name our Domain Controller. Start Menu > Search for "Rename"

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(224).png)

Click "Rename this PC"

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(692).png)

Rename it to what you see fit

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(356).png)

Restart the VM

Our Dashboard is showing us a "Warning" Dialog

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(191).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(199).png)

Click Promote this server to a domain controller

We're going to add a new Forest and give it a name

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(349).png)

Give the DSRM a password. I set it the same as the Admin password. (Not the most secure, but this is for a lab environment)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(211).png)

Then from here it'll be standard Next to Install

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(566).png)

This screen may take a min to finish loading

![](https://github.com/Th4ntis/CyberSecNotes/blob/main/.gitbook/assets/image%20(378).png)

Now we install it

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(336).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(439).png)

Once done will automatically reboot. After reboot and login, we should now see

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(530).png)

After we Setup user machine and connect them to the Domain, we will do more.

# Post Setup Setup

Now that we have the basics out of the way, lets add some users, groups, and policies.

From the Server Manager Dashboard > Tools (Top Right) > Active Directory Users and Computers

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(177).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(197).png)

I'm going to make a separate Organizational Unit (OU) for Groups just for organization.

Right Click on the Domain.local > New > Organizational Unit

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(619).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(469).png)

Then Select all the Groups and Drag them into the Groups Folder, so there should only be Administrator and Guest in the Users folder and all the groups in the Groups folder. Easy.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(512).png)

So let's add our Users. Right click in a blank area in Users > New > User

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(457).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(154).png)

Add some users and choose the naming convention. For my example my user doesn't have a last name but if you choose to and want to have a naming convention of first.last or FLast or FirstL, this is where you do that.&#x20;

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(537).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(380).png)

Give the user a password, uncheck making the user change the password, and set the password to never expire. This is a VERY bad practice but for a lab environment things will setup a little different than in the real world, at least we hope.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(448).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(606).png)

Make another user BUT instead of creating it, right click on Administrator, copy it, and follow the same procedure. This will create a second Domain Admin account.

Create a second base normal user as well.

Now we're also going to setup a file share. From the Server Manager Dashboard > File and Storage Services (Left Hand Side) > Shares

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(431).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(747).png)

From the top click on Tasks drop down and select new share > SMB Share - Quick > Next

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(609).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(333).png)

Give the share a name

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(719).png)

Then just click next till it asks you to create it, create the share.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(264).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(308).png)

Now we are done!

# Optional: Disable Windows Defender

IF you want the other VMs that will join this Domain to have Windows Defender disabled, I recommend doing this for simplicity sake, IF you are pentesting against this and having another VM setup with Defender Enabled to test things against that.

Start Menu > Group Policy > Right Click and Run as Admin

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(766).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(613).png)

Right click on out domain and Create a new Group Policy in this domain (Top option).

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(665).png)

Name this "Disable Windows Defender"

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(309).png)

Right click on newly added "Disable Windows Defender" GPO on the left and Edit it

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(231).png)

Drill Down: Computer Configuration > Policies > Administrative Templates > Windows Components > Windows Defender Antivirus

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(382).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(470).png)

Double click on the "Turn off Windows Defender Antivirus" > Enabled > Apply > Ok

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(266).png)

Close out of the Group Policy Management Editor and on the Group Policy Management Window, with the Disable Windows Defender selected, if 'Enforced' says no, right click on it, and enforce it

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(679).png)

Now we are done!

Again, I **HIGHLY** recommend creating a snapshot after you have this done and setup so that way you can always revert back to that snapshot if needed if something breaks or you just need to clean things up.

