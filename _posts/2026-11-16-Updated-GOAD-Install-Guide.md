---
layout: post
title: Updated GOAD Install Guide
categories: guide
author:
  - th4ntis
---
# Installing
Once we have [installed Ludus](https://th4ntis.com/guide/2026/11/16/Updated-Ludus-Install-Guide.html) onto our machine, we can proceed with installing [Game of Active Directory (GOAD)](https://orange-cyberdefense.github.io/GOAD). This will give us a great environment to attack, test tools against, and more.
## CLI
To install GOAD via CLI we need to add the source first. Pressing space to select `Game of Active Directory (GOAD)`, then press `enter`.
```
ludus source add ludus-source-bsl
```
<img width="922" height="489" alt="image" src="https://github.com/user-attachments/assets/7b71622c-2afe-4aab-a397-d18f965541ed" />


Create the range, user,  and select the blueprint
```
ludus range create -r GOAD --name GOAD --user th4ntis-ludus --from-blueprint ludus-source-bsl/goad
```

Then deploy the range.
```
ludus range deploy -r GOAD
```

> [!note] 
> This will take some time, so step away for a few. This also may fail, but can be selected to be deployed again.

We can view the deploy progress with:
```
ludus range logs -f -r GOAD
```
## Web UI
Installing GOAD from the source blueprints, we simply select `Sources`, select the `Ludus Source - Bad Sector Labs` source.
<img width="1309" height="472" alt="image" src="https://github.com/user-attachments/assets/f49ba8d1-7b51-4809-b983-fdb519c2ee62" />

To install GOAD we select `Blueprints`, then check box, and hit install.
<img width="1529" height="588" alt="image" src="https://github.com/user-attachments/assets/fb036e69-6d1d-4f8b-b977-51bc5697f3a0" />

Now we go to Ranges and make a new one, and name it. I went with GOAD, then select `Create Range`
<img width="1144" height="472" alt="image" src="https://github.com/user-attachments/assets/890fb982-5815-46be-b8a2-bde6d972998f" />

<img width="530" height="582" alt="image" src="https://github.com/user-attachments/assets/a54226d7-07de-4ac2-9fb1-63503a7a0ce6" />

Now select `Blueprints` from the side panel, and you should see `Game of Active Directory (GOAD)`. Select the 3 dots and apply it to a range.
<img width="1145" height="552" alt="image" src="https://github.com/user-attachments/assets/58650cdd-0a21-4e4b-87a8-86a65bdfa4e4" />

Select the Target Range we created, and select `Apply Blueprint`.
<img width="925" height="742" alt="image" src="https://github.com/user-attachments/assets/13292615-1be2-43db-8cf9-e8a19757093a" />

This will take us right into our range, and give us a run down of configuration canvas and show us the layout. From here, to deploy the range, we select the Play button icon in the top right. This will take some time.

> [!note] 
> This will take some time, so step away for a few. This also may fail, but can be selected to be deployed again.

<img width="1148" height="822" alt="image" src="https://github.com/user-attachments/assets/d4a540f7-cf51-4902-9df6-36dccb2dac9d" />

# Connection
Now that GOAD is installed and running, we need to access it. We can do this by getting the WireGuard config.
## CLI
With our API key exported, we can run:
```
ludus user wireguard | tee ludus.conf
```
<img width="574" height="238" alt="image" src="https://github.com/user-attachments/assets/9d23c5cd-ba7d-4008-8f44-d3765f0e3bca" />

From here, we copy/paste that into a new.conf file.
## Web UI
When on any page, we can select our name in bottom right, then `WireGuard Config`
<img width="459" height="230" alt="image" src="https://github.com/user-attachments/assets/76ff97a4-1597-4e38-85d0-1f1fed5af853" />

With that, we can either copy or download it.
<img width="533" height="394" alt="image" src="https://github.com/user-attachments/assets/a0e60aef-ec92-4f4d-a7b6-4f3fe73e15dc" />

From our machine we use the .conf file to connect to the environment.
For Linux it's under `/etc/wireguard/` so we can run:
```
sudo mv wireguard.conf /etc/wireguard/wireguard.conf
```

# Access
We now connect to the Kali machine (or the other machines) with
```
sudo wg-quick up wireguard
```
<img width="518" height="167" alt="image" src="https://github.com/user-attachments/assets/6c050bdf-0a53-4bef-8222-2b50c5ee0d38" />

We verify by pinging our GOAD Kali machine
<img width="628" height="210" alt="image" src="https://github.com/user-attachments/assets/7e6d1b5f-f0db-4b0b-aecc-071885e09f40" />

From here, we can SSH to the machine or if we want a GUI we can go to `https://10.1.10.99:8444` and login with `kali:kali`
<img width="923" height="427" alt="image" src="https://github.com/user-attachments/assets/77341a64-60b5-48d2-8492-798c3a69aaf1" />

# Power
## CLI
```bash
ludus power on/off --name all -r RANGE_ID # Power on/off all range VMs
```
## WebUI
To power on/off the VMs, we either right-click anywhere within the range editor and select `power on/off All VMs` .
<img width="741" height="852" alt="image" src="https://github.com/user-attachments/assets/58a6b9c3-d795-4ff7-96f6-02e83d49dfbd" />

 Or we can click the three dots in the top right, go to power, and select power on/off.
<img width="432" height="186" alt="image" src="https://github.com/user-attachments/assets/5cd82cbd-ba7f-48b6-8e4f-6a0b695ef26e" />

# Conclusion
With GOAD installed and having access, we have a great Active Directory (AD) environment to safely test tools/tooling, tactics, techniques, and procedures, and more! Stay tuned for my `Attacking GOAD` posts coming soon!
