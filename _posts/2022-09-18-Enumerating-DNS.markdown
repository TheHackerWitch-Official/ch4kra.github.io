---
layout: post
title: "Digging For Gold: DNS Enumeration"
date: 2022-09-25 09:00:00 -0500
description: DNS is the backbone of the Internet. Read this post when you're ready to up your DNS enumeration skills! 
img: dns-enum/header.png
tags: [Enumeration, DNS, Penetration Testing, Nmap]
---
DNS is the backbone of the Internet; knowing how to enumerate it is a necessary skill for anyone interested in Information Security or networking. Today, we'll be exploring my favorite ways to probe DNS. 

Let's get into it!

# What Is DNS?
DNS stands for Domain Name System. At a high-level, it works like the Internet's phonebook. I like to think about it like this: humans navigate the Internet using domain names (ex. google.com). These domains have associated IP addresses (ex. 142.250.74.14). DNS translates domain names to IP addresses to make the Internet easily navigable.

The default port for DNS is 53.

# Why You Should Enumerate DNS
DNS should be enumerated during every pen test because it can reveal the true extent of an organization's attack surface. By enumerating DNS, you can gather information and sub-domains that are not available through other enumeration methods. Having a clear idea of the attack surface will save you time and aid you in developing a methodical plan of attack.

So, without further ado, let's enumerate! I will be using `hackthebox.eu` for this exercise.


## Finding Mail Servers 
To find mail servers, use the *host* command:
```console
host -t mx hackthebox.eu
```

**Example: Using The host Command To Find Mail Servers**

![Find Mail Servers]({{site.baseurl}}/assets/img/dns-enum/find_mail_servers.png)

Alternatively, the *dig* command can be used:
```console
dig mx hackthebox.eu
```

## Finding Name Servers 
To find nameservers, use the *host* command:
```console
host -t ns hackthebox.eu
```

**Example: Using The host Command To Find Name Servers**

![Find Nameservers]({{site.baseurl}}/assets/img/dns-enum/find_nameservers.png)

Once again, the *dig* command can be used:
```console
dig ns hackthebox.eu
```

## Finding Subdomains
The following bash script can be used to enumerate subdomains.
```console
for sub in $(cat /path/to/wordlist);do dig $sub.domain.com @DNS_IP | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subs.txt;done
```

*Note: there are many great tools for subdomain enumeration. Enumerating DNS to find subdomains can be a bit clunky. Use as needed*


## Useful Nmap Scripts
Nmap is equipped with tons of tools to make your life easier. These are my favorites for enumerating DNS.

**To enumerate common service records (SRV):**
```console
nmap --script dns-srv-enum --script-args "dns-srv-enum.domain='DOMAIN.COM'"
```

**To enumerate DNS names (also useful for subdomains):**
```console
sudo nmap -sSU -p 53 TARGET --script dns-nsec-enum --script-args dns-nsec-enum.domains=DOMAIN.COM
```

## Other Useful Tools 
- [DNS Dumpster](https://dnsdumpster.com)
- dnsrecon
- subscraper

I hope you found this post useful! Thanks for giving it a read. Happy hacking! :) 

