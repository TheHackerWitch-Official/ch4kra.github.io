---
layout: post
title: "Five Offensive Security Tools For Beginners"
date: 2022-07-21 00:00:00 -0500
description: New to Offensive Security? Learn about 5 tools to get a jump start on your hacker journey! 
tags: [Blog, InfoSec, Offensive, Tooling, victoriamgalvan, The Hacker Witch, Beginner Friendly, Burp Suite, Nmap, Nuclei, John The Ripper, Metasploit]
---

In this post, you'll be introduced to five essential offensive security tools that will provide great groundwork for a career in information security. While not extensive, this list is diverse and a great place to start learning about tooling! Let's jump into it!  


## 1. Burp Suite
**Price:** Free - $400/year \\
**Publisher:** PortSwigger \\
**Primary Use Cases:** Web application scanning, intercepting and modifying requests, fuzzing 

Known as the hacker's Swiss Army Knife, Burp Suite is a penetration testing framework designed to aid mobile and web application assessments. It has powerful proxying, scoping, and attacking features that make it invaluable during engagements.

Burp Suite comes in two forms: 
- Community (Free)
- Pro ($400/year)

I recommend downloading the Community edition and setting up Burp's proxy (follow [this](https://portswigger.net/burp/documentation/desktop/tools/proxy/getting-started) guide). Like all the tools in this post, the best way to learn Burp is by using it. I learned by using Burp to analyze web traffic on retired Hack The Box machines, but do what works best for you! There is no "right" way to learn. 

[Get BurpSuite](https://portswigger.net/burp/communitydownload)



## 2. Nmap
**Price:** Free \\
**Creator:** Gordon Lyon (aka Fyodor Vaskovich) \\
**Primary Use Cases:** Port scanning, host discovery, version identification

Nmap, short for "Network Mapper," is a command line utility used for network discovery and security auditing. Nmap is a great tool for discovering open ports, operating systems, and firewalls. It is one of the first tools I use when conducting network enumeration. Nmap gives you a really good idea of a given host's attack surface and is simple to use. 

[This](https://www.stationx.net/nmap-cheat-sheet/) is my favorite nmap cheatsheet. Give this tool a go the next time you need to gently (or not so gently) scan a network. Excellent enumeration is the key to a fantastic engagement. 

Pro Tip: use `-oX filename` to save nmap results to an XML file.  

[Get Nmap](https://nmap.org/download)



## 3. Nuclei 
**Price:** Free \\
**Publisher:** Project Discovery \\
**Primary Use Cases:** Vulnerability scanning 

Nuclei is an efficient vulnerability scanner written in Go. Users can create or use default YAML templates to quickly scan hosts for known vulnerabilities. Nuclei is excellent for quickly scanning target systems and obtaining useful output. 

[Get Nuclei](https://github.com/projectdiscovery/nuclei)



## 4. John The Ripper 
**Price:** Free \\
**Publisher:** Openwall \\
**Primary Use Cases:** Password cracking

John the Ripper is an insanely powerful password cracking utility. It is an *essential* part of any hacker's toolbox. John can conduct a variety of password attacks, supports hundreds of hash and cipher types, and is a hands-down fun tool to use. John is also great for cracking compressed file types like .zip and .rar. It is a one-size-fits-all password cracker and a **must-know** tool. 

[Get John The Ripper](https://www.openwall.com/john/)



## 5. Metasploit Framework
**Price:** Free - $$$ \\
**Publisher:** Rapid7 \\
**Primary Use Cases:** Vulnerability exploitation, command and control, vulnerbility scanning

Metasploit is one of the most powerful penetration testing tools you can add to your toolkit. It is a penetration testing platform that allows researchers to find, exploit, and learn about vulnerabilities. Metasploit can feel like "cheating" because of how simple it is to use. Gaining a foothold can be as easy as searching for a vulnerability, staging it in Metasploit, and clicking Enter. 

Metasploit is excellent and efficient, but *please* research vulnerabilities before exploiting them. It is important to understand what an exploit does before using it. I get it, exploits are rarely simple to read, but Metasploit + a bit of research will teach you far more than Metasploit alone.  

[Get Metasploit Framework](https://www.metasploit.com/download)



Thanks for giving this post a read! If you found it useful, consider giving it a share :) Until next time, hackers! 

