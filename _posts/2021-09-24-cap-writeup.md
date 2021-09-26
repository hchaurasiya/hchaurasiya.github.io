---
layout: post
current: post
navigation: True
title: Cap Writeup
date: 2021-09-24 12:00:00
cover: assets/images/cap_logo.png
class: post-template
subclass: 'post'
author: r3Z3l
---

Easy Linux box from HackTheBox.

On rustscan we found It has 3 ports open 21,22,80.

first tried to use filezilla to login ftp but couldn't.
![Pasted image 20210615144701.png](assets/images/Pasted image 20210615144701.png)
I needed credentials to login ftp.

Let's Enumerate webpage. I found it's giving packets.
![Pasted image 20210615160539.png](assets/images/Pasted image 20210615160539.png)

Let's try to check data packet 0.
using wireshark.
![Pasted image 20210614120303.png](assets/images/Pasted image 20210614120303.png)
we got credentials.
user **Nathan** 
password **Buck3tH4TF0RM3!** 
We can ssh and ftp using these creds.
on doing ssh we get #user-txt 

For root priv let's run linpeas.sh
We found that python has capabillity to setuid as 0.

![Pasted image 20210615144316.png](assets/images/Pasted image 20210615144316.png)

so run ```python3 -c 'import os; os.setuid(0);os.system("/bin/sh")'```


and we get root shell and finally #root-txt 

