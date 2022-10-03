---
layout: post
title: "HTB Writeup: baby BoneChewerCon"
date: 2022-07-24 09:00:00 -0500
description: Baby BoneChewerCon is a HackTheBox challenge that involves parsing information from a debugger.
img: bonechewer/header.jpg
tags: [HTB, Writeup, baby BoneChewerCon, debugger, OWASP Top 10]
---

To solve this challenge, navigate to the webpage and enter any data in the input box. I simply put "The Hacker Witch."

Press "Register Me"

![Press "Register Me"]({{site.baseurl}}/assets/img/bonechewer/input-bb-bonechewer.png)


This will lead you to an error page. 

![Error Page]({{site.baseurl}}/assets/img/bonechewer/error-bb-bonechewer.png)

Scroll through the error message until you see the `APP_KEY` field. That's your flag! 

![Flag]({{site.baseurl}}/assets/img/bonechewer/flag-bb-bonechewer.png)

Happy hacking :) 



