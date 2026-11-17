---
layout: post
title: Updated Install Ludus Guide
date: date: 21:00:00 -0400
categories: guide
author:
  - th4ntis
---
With some new and awesome changes within [Ludus](https://docs.ludus.cloud/docs/intro/), I wanted to create a new post covering what it is, and using it. I will be making a post about installing GOAD shortly.

[Ludus](https://docs.ludus.cloud/docs/intro/) is a system to build easy to use cyber environments, or "ranges" for testing and development. Built on Proxmox, it enables advanced automation while still allowing easy manual modifications or setup of virtual machines and networks. It is implemented as a server that runs Packer and Ansible to create templates and deploy complex cyber environments from a single configuration file. It is accessed via the Ludus CLI (client), the WebUI, or the Proxmox web interface. Normal users should not need to access Ludus via SSH.

## Links
[Homepage](https://ludus.cloud/)
[Docs](https://docs.ludus.cloud/docs/intro/)
[Install Ludus](https://docs.ludus.cloud/docs/quick-start/install-ludus/)
[Changelog](https://ludus.cloud/changelog)
# Requirements
Ludus does have specific system requirements to be used. I've highlighted some specifics. 
<img width="1041" height="541" alt="image" src="https://github.com/user-attachments/assets/f19f0829-33ed-4667-90a3-124b2ae9894f" />

I'm installing mine on an HP EliteDesk DM running [Debian 13](https://www.debian.org/download)) with:
- Intel i5-8500T@ 3.50 GHz
- 32GB RAM
- 1TB NVMe SSD

# Install
To install ludus we will need to be the root user on our Debian machine
```
su -
```

Then we install pre requisites
```
apt update && apt install curl sudo ca-certificates
```
<img width="1120" height="610" alt="image" src="https://github.com/user-attachments/assets/29796eca-efac-439f-920a-66c77f037118" />

Now we install Ludus with
```
curl --proto '=https' --tlsv1.2 -sSf https://ludus.cloud/install | bash
```
<img width="1919" height="975" alt="image" src="https://github.com/user-attachments/assets/32a68160-528a-4998-a144-d2522913bd59" />

We accept the license agreement
<img width="1492" height="915" alt="image" src="https://github.com/user-attachments/assets/e64c5f87-df35-43e9-ae96-45a799442a27" />

This will ask some questions, have you accept an EULA and go through some setup config. Most, if not all, can be kept default.
<img width="878" height="574" alt="image" src="https://github.com/user-attachments/assets/d3472787-6b13-4fc7-b6f6-ae0da0ddf9f3" />

This will also prompt you to make an admin user.
<img width="874" height="578" alt="image" src="https://github.com/user-attachments/assets/1096fb66-b3ec-440d-8cf7-a4db6f2c03e8" />

Then will ask if you're ready to install and proceed when you select yes. 

> [!note] 
> This will take some time, so step away for a few. The machine may restart once or twice as well. With the newest install, it pauses for the reboot as to not drop the SSH connection.

<img width="570" height="328" alt="image" src="https://github.com/user-attachments/assets/5ed07721-ba4e-4602-8e2e-7ea5dfea91b4" />

Once you see the following, it should be installed.
<img width="1163" height="320" alt="image" src="https://github.com/user-attachments/assets/c988c446-e691-4f01-8452-571e526bb3f4" />

We can view our install status, creds, and API key with
```
ludus-install-status
```
<img width="1060" height="281" alt="image" src="https://github.com/user-attachments/assets/cbff581c-4476-45c5-8543-378dc6a24d5a" />

> [!note]
> If running this headless, don't forget to set the API key with

```
export LUDUS_API_KEY='API-KEY-HERE'
```

## Adding License Key
If you get a license key or an NFR (Not For Resale) license, we we can add it during install or after install. Your key should look like:
```
46C1CA-B11C52-80E9E7-19E436-FDDG1B-V3
```

You will need to add the key to your Ludus Config file. Mine was under the default of `/opt/ludus/config.yml`. Inside there will be a spot for `license_key: `. Without a key  it will be `community`. Remove `community` and paste in your key and save.

Once that is done we run the following to set the Web URL
```
LUDUS_URL=https://LUDUS_MACHINE_IP:8080
```

Then we restart the ludus service
```
systemctl restart ludus
systemctl restart ludus-admin
```

# Sources
## CLI
To view current sources we run:
```
ludus source list
```

We can see what is within a source such as blueprint, templates, and ansible, To view the catalog within a selected source we run:
```
ludus source list ludus-source-bsl --catalog
```
<img width="1237" height="932" alt="image" src="https://github.com/user-attachments/assets/e49cc35f-f636-4c6d-8019-49d0b9dc2b9f" />

I'm only going to cover the official ludus source but you can find out more [here on their docs](https://docs.ludus.cloud/docs/using-ludus/sources/).
## Web UI
From the Web UI we can simply click on 'Sources' on the left and view, or add sources.
<img width="828" height="472" alt="image" src="https://github.com/user-attachments/assets/65b202eb-633d-4ca0-b393-1e9af846a647" />

To view the catalog from that source, just select the source.
<img width="1014" height="775" alt="image" src="https://github.com/user-attachments/assets/dfe03c96-1420-422c-b3a0-b53bd8f7298e" />

# Templates
## CLI
We can see what templates are included by default, and if they are built. To view this we run:
```
ludus templates list
```
<img width="576" height="202" alt="image" src="https://github.com/user-attachments/assets/ba1ffc14-67a5-4d9d-8bb8-f9c5cf2088c7" />

To add a template we run
```
ludus source add ludus-source-bsl --templates TEMPLATE-NAME
```
Eg. Debian 13
```
ludus source add ludus-source-bsl --templates debian-13-x64-server-template
```
<img width="809" height="278" alt="image" src="https://github.com/user-attachments/assets/d792589c-10e8-47ba-b424-ddd2a373c634" />

To build a template we run:
```
ludus templates build -n debian-13-x64-server-template
```

or we can build them all with:
```
ludus templates build
```

> [!note] 
> This will take some time, so step away for a few. The machine may restart once or twice as well. With the newest install, it pauses for the reboot as to not drop the SSH connection.

<img width="846" height="70" alt="image" src="https://github.com/user-attachments/assets/ce3715b8-1204-4519-bf68-85a86db3e06b" />

To check on the status, this will show us where it's at in the process and when it's finished. We can run:
```
ludus templates logs -f
```
<img width="1018" height="309" alt="image" src="https://github.com/user-attachments/assets/d2dd6988-166c-449b-877c-f568ff3fd5af" />

<img width="1016" height="637" alt="image" src="https://github.com/user-attachments/assets/57593c35-053f-4f8d-94e0-2bcb4fabbd4c" />

I'm only covering what is included with the base ludus, but more info on Templates can be found [here on their docs](https://docs.ludus.cloud/docs/using-ludus/templates).
## WebUI
Selecting `Templates` on the side panel will show us current templates and their status.
<img width="1313" height="813" alt="image" src="https://github.com/user-attachments/assets/717d79b5-c569-424a-b338-67885da80342" />

If we want to add more templates, we need to select `Sources` from the side panel then select our source.
<img width="1309" height="472" alt="image" src="https://github.com/user-attachments/assets/d39951c2-2840-47fc-8662-2c64e63456e4" />

Within here we can see blueprints to build, templates, and ansible roles. Selecting `Templates`, we can check the boxes of the templates we want to add and click install.

> [!note] 
> This will take some time, so step away for a few. This also may fail, but can be selected to be built again.

<img width="1525" height="999" alt="image" src="https://github.com/user-attachments/assets/cff1e954-aedd-4ecd-84d9-e9b5a003c780" />

# Ranges
## CLI
Once your API key exported
```
export LUDUS_API_KEY='USER-API-KEY'
```

See what ranges you have access to, note the `RANGE ID`
```
ludus range accessible
```

View the status of the range
```
ludus range status -r RANGE_ID
```
<img width="1090" height="485" alt="image" src="https://github.com/user-attachments/assets/88d6ed68-82c1-457a-bb21-b85235b49896" />

# Power
## CLI
```bash
ludus power on/off --name all -r RANGE_ID # Power on/off all range VMs
```
## WebUI
To power on/off the VMs, we either right-click anywhere within the range editor and select `power on/off All VMs` .
<img width="741" height="852" alt="image" src="https://github.com/user-attachments/assets/44d6be21-3670-4c7c-87e8-7bca0f9284f0" />

 Or we can click the three dots in the top right, go to power, and select power on/off.
<img width="432" height="186" alt="image" src="https://github.com/user-attachments/assets/f2738f64-c6d6-42c5-a77d-75c95c9bc300" />

# Conclusion
That's it for now covering he basics of what Ludus is, installing it and the common usage. Be sure to check out my guide on [installing Game of Active Directory](https://th4ntis.com/guide/2026/11/16/Updated-GOAD-Install-Guide.html) for a great AD environment to test TTPs, APTs, and other tooling against.
