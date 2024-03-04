---
layout: post
title: "Setting Up an Ubuntu VM"
categories: guide
author:
- th4ntis
---

A virtual machine(VM) is the virtualization or emulation of a computer system. It's a way to run a computer system such as windows or linux. You can use this to test new/other features, software, configurations, etc. 

The linux distribution [Ubuntu](https://ubuntu.com/) is usually a good first linux distro for most users as it has LTS(Long time support) has most drivers for hardware included or easy to obtain, and it usually stable. I usually go with [Kubuntu](https://kubuntu.org/) as it has the K Desktop Environemtn (KDE) which is very lightweight so it won't take up as many resouces with background processes, as well as it's highly customizable to make it look and feel however you like..

VM software that is used to emulate other computer systems are [VMWare Workstation Player](https://www.vmware.com/products/workstation-player.html) (Free version), [VMWare Workstation Pro](https://www.vmware.com/products/workstation-pro.html) (Paid version), or [Virtualbox](https://www.virtualbox.org/). I have VMWare Workstation Pro but Player works just as well. You can go with either VMWare or Virtualbox, both work and it just comes down to personal preference on the application and what you're use to. There's minor differences between the two for most end users.

Obviously your setup may differ depending on your system specs. I typically go with 4GB(4096 MB) of RAM per VM, 2 processors and 2 cores per processor but I have a bit beefier machine. If you need to, you can start with 4 or 8GB of RAM and 2 processors and 2 core per processor, for the install so it goes faster then drop it down to 2 or 4GB of RAM and 2 processors and 1 core per processor.

# *buntu install

We will start with a typical configuration

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FtEq66e36Kz756lDXZN9p%2Fimage.png?alt=media&token=990847a2-8d11-4ce1-84d4-d8b0d1114d6d)

Then install choose "I will install the Operating System Later"

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FT06m6EZ7m9k3KaeUzGxe%2Fimage.png?alt=media&token=4948ded6-3a64-4648-aa5a-ef6930b91b03)

Select Linux, then select the Ubuntu 64-bit version.

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FQBabDlPSdIw5TjggBUum%2Fimage.png?alt=media&token=2f724043-29dc-4ce4-8f48-a227e5e2730b)

Name it and choose a location to store the VM files

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FYZYq28JXzAr3rzwfBnIl%2Fimage.png?alt=media&token=dd29f4b3-da1d-453d-8767-dc6c3e7510f6)

Select the size of the VM. This will **NOT** the overall size, this is just the max size of the VMs HDD space and will fill up as we add more to the VM that takes up space.

If this VM will be on a PC and not be used from an external HDD or moved around you can store it as a single file but if you plan on using this VM on other PC or from an external HDD it's a better idea to split it into multiple files.

Depending on your space, you can edit how much you want. I usually go 60GB or 80GB depending.

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FyGGZ572aHlnBNZWEa0J7%2Fimage.png?alt=media&token=8bb59d55-5765-4e93-a350-337700db3284)

Finally we can now customize our hardware. This is where we can customize the RAM, Processors, ISO files, Network Settings, etc. This is where we select our Ubuntu ISO.

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FvEAs5fz7wvqU77sreM3D%2Fimage.png?alt=media&token=7cf11b8d-f02c-42b4-b3fb-06c8182371ed)

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FqGmXJe4yhGHzTd9nGl5O%2Fimage.png?alt=media&token=7fa8fb5e-fd41-4a55-8b7d-35882a46b865)

Now we close that and can start our VM and install it. This process will be VERY similar for each desktop environment just may look different. Once we start the VM, itll take us to a list, we can select the first option or just let it auto select it after 10 seconds.

So from here we select `Install *buntu`

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FRqC9Ke4ypMGKVhkLj5ws%2Fimage.png?alt=media&token=4630c256-37b3-428a-b183-dfce10b2de3b)

Select language and keyboard layout

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FTLEEp8DGUNMrARsCUmVg%2Fimage.png?alt=media&token=a277bf22-b702-450d-a6bd-647cf0424e9b)

Make sure you tick 'install third-party software for graphics..." and such to ensure this includes drivers you may need for your hardware.

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FqhjCQRZqSLhQFQCPOPjF%2Fimage.png?alt=media&token=f93802dd-243c-4dd2-bbfa-5c2a60f0bb6b)

As this is on a VM, this default option is ok for this.

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FmKSyuyGpLVizZsqA6vQN%2Fimage.png?alt=media&token=44bf28dd-951b-42dc-a415-68b302b8d883)

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2F8LR2R5mGld8bMnjPdeVL%2Fimage.png?alt=media&token=6a2d48c3-4613-4ae2-bc97-8f3928a3d91b)

Select Timezone

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2F9dpIprlYgGbnuL1pbrXc%2Fimage.png?alt=media&token=8c8f8244-1992-440e-a2d0-fbed7337a26b)

Setup the username, hostname, and password

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FypZa2ZQ8BDoYYurQijr4%2Fimage.png?alt=media&token=3422578c-a28b-4832-9f45-5f93cbb54ddc)

Let the install finish

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FouhVebDPUJFyVeHGZUBE%2Fimage.png?alt=media&token=bde383dd-825e-4623-8b28-5ba8b5468cf1)

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FHaR3Y10LIvAeXlKaOHd4%2Fimage.png?alt=media&token=084eec54-1979-4231-b396-dfbcc975fd59)

If it asks you to "Remove the installation media and press ENTER" just press `ENTER`.

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FZU76ra1B1WSk2MsnQkqc%2Fimage.png?alt=media&token=6b7d2b8b-2b29-49f4-8827-a9798a0d7547)

From here it will restart and you will be taken to a login screen. So login and ta-da!

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FxLJejEJUeqWXCcJTzwRP%2Fimage.png?alt=media&token=9dbc6699-0afd-49cb-8914-97a1ee2a2022)

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2Fsu6pjjid3YYuLj3cqTT0%2Fimage.png?alt=media&token=ccc7e9e8-5fc6-4b59-8b57-3ed9fbc820f2)

Depending which Desktop Environment (DE) you chose, you're may look different but that is ok, it's still the same system under the hood.

Once we're in, it's a good idea to open the terminal, in Kubuntu it's called `Konsole`, and we update everything with:
```
sudo apt update && sudo apt upgrade -y
```

Now is when you will want to shut down the VM and adjust RAM and Processors if needed. Drop it for 2GB or 4GB of RAM and 1 or 2 processors.

![Picture](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FTdW22AGCceN8oUXfdlKI%2Fuploads%2FsczPIu97Xw6Hykc2Aw5i%2Fimage.png?alt=media&token=916f0ae1-b053-4f02-a061-b941a327e70d)

From here you can explore and find things, change settings, and learn. I **HIGHLY** recommend creating a snapshot after you have this done and setup so that way you can always revert back to that snapshot if needed if something breaks or you just need to clean things up.
