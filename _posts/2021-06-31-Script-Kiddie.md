---
layout: post
current: post
navigation: True
title: Script Kiddie Writeup
date: 2021-07-01 20:00:00
cover: assets/images/scriptkiddie_logo.png
class: post-template
subclass: 'post'
author: r3Z3l
---

Easy Linux box from HackTheBox.

Starting with port scanning.
let's use rustscan.
![Pasted image 20210603125017.png](assets/images/Pasted image 20210603125017.png)
It has 2 ports open 22 and 5000.

Let's visit port 5000.
Webpage give three functionality.
- port scanning
- msf payload generator
- sploit search

On, searching of msfvenom exploit we found apk template injection.

Let's generate reverse shell payload using msfconsole.
open up msfconsole and search msfvenom.
![Pasted image 20210603125802.png](assets/images/Pasted image 20210603125802.png)
let's use it.
set your lhost and lport and then run.

We got exploit let's try to upload it on payload page.
![Pasted image 20210603130240.png](assets/images/Pasted image 20210603130240.png)
It give us a reverse shell.
and make it stable using python tty command.
```python3 -c 'import pty; pty.spawn("/bin/bash")'```


We found the user.txt in kid's directory.

On enumeration we found that there is a file scanlosers.sh that is running every time.
let's read scanlosers.sh
![Pasted image 20210603132011.png](assets/images/Pasted image 20210603132011.png)

It's reading from a hackers file which is in kid's logs directory and we have permission to write on this file.
Let's copy code on your machine to get exploit.

Let's try to edit a file in such a way that we can get reverse shell.
![Pasted image 20210603132636.png](assets/images/Pasted image 20210603132636.png)
testing on my machine i found that it's ignoring first two written words.
so i tried to edit hackers file so i can get reverse shell using nc.


Edited hackers file with this :
![Pasted image 20210603145816.png](assets/images/Pasted image 20210603145816.png)

when i saved this file i got reverse shell as pwn user.

On, checking sudo -l , I found that metasploit can be run as root without password.
![Pasted image 20210603150149.png](assets/images/Pasted image 20210603150149.png)

run:
```sudo msfconsole```

then run:
```bash```
and we got root shell.
Now, we can cat /root/root.txt
