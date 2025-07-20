---
layout: post
title: "Phases of Penetration Testing"
source: "https://anotsodev.me/phases-of-penetration-testing/"
author:
  - "[[anotsodev]]"
date: 2017-08-24
description: "Penetration Testing or Pentesting is a process where the testers will assess and discover the vulnerabilities that are present in a network or system and will exploit these vulnerabilities to gain …"
tags:
  - "clippings"
---
Penetration Testing or Pentesting is a process where the testers will assess and discover the vulnerabilities that are present in a network or system and will exploit these vulnerabilities to gain access. Pentesting is also the process of simulating real world scenario where the testers will act and think like an attacker to assess and discover weaknesses and exploit those weaknesses of the target system.

There are a lot of topics in penetration testing but I will only discuss the phases and techniques that I usually use when I am attacking a vulnerable system.

In general, the phases of penetration testing are

1. Reconnaissance
2. Scanning
3. Gaining Access
4. Maintaining Access
5. Covering Tracks

Before attacking our target system, we always need to plan our moves to get a higher chance of gaining access to the system.

## Reconnaissance

So first is the Reconnaissance. Reconnaissance is the phase where we gather available information of the target with the use of search engines and/or social media sites. There are two types of reconnaissance, the passive reconnaissance, and active reconnaissance.

**Passive reconnaissance** is the gathering of information on Google, company profile, social media, name servers, etc.

**Active reconnaissance** is the gathering of information through scanning the target system with the use of tools that are available and can be downloaded online.

Beware of the IDS/IPS and Firewall when scanning because we are most likely will fail if the target system blocked our access.

Tools for **active reconnaissance**

- nmap
- maltego
- etc.

The goal of this phase is to gather the initial information of the target.

## Scanning

After we gather the available information of the target, we will scan the target system to discover vulnerabilities that are present for us to exploit.

Tools for scanning

- nmap
- auxiliary modules in metasploit
- etc.

Again, beware of the IDS/IPS and Firewall when scanning.

The goal of this phase is to have an idea on how we can exploit the vulnerabilities that are present on the target system. For example, if the target system is vulnerable to a certain exploit that is available in public, we can add this to our options in exploiting the target system.

## Gaining Access

After enumerating our target, we will now move on to the fun part in hacking; the exploitation phase.

- Proper enumeration can lead to more chance of exploitation.
- You can use public exploits to attack the vulnerable services that are present on the target’s local machine.
- Public exploits may not work out of the box so you need to analyze how the exploit works by tracing the source code of the exploit.
- Modify the source code of the exploit if you need to.
- In compiling public exploits locally, make sure that it matches the kernel version of the local machine to the target machine.

The goal of this phase is to properly exploit the vulnerabilities of the target and gain low-level or privileged access to the system.

## Maintaining Access

When we successfully exploited and got a low privileged access to the system, we need to maintain our access long enough to achieve our goal.

- If you have gained access to the system, you may install a backdoor in the case of the user reboots the target machine
- Since you only have an initial access to the system (low privileged access), enumerate for the available programs, weak credentials, default passwords, and others that may be used to escalate your privileges.
- Check for the versions of the available services that are present on the target’s local machine
- You can use automated scripts to automate the enumeration process
- - **Linux Privilege Escalation Enumeration Scripts**
		- unix-privesc-check – [http://pentestmonkey.net/tools/audit/unix-privesc-check](http://pentestmonkey.net/tools/audit/unix-privesc-check)
		- linuxprivchecker.py – [https://gist.github.com/sh1n0b1/e2e1a5f63fbec3706123](https://gist.github.com/sh1n0b1/e2e1a5f63fbec3706123)
		- linenum – [https://www.rebootuser.com/?p=1758](https://www.rebootuser.com/?p=1758)
		- linux-local-enumeration-script.sh – [https://highon.coffee/blog/linux-local-enumeration-script/](https://highon.coffee/blog/linux-local-enumeration-script/)
		- Basic Linux Priv Escalation(blog post) – [https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)

The goal of this phase is to maintain and gain system level access to the system.

## Covering Tracks

After we have gained access to the system and we already achieved our goal, we need to delete all the backdoors, exploits, and other files that we used to gain access to the system. We also need to delete or modify the system logs to cover our tracks.

The goal of this phase is self-explanatory.

## Conclusion

So there are five phases of penetration testing and these are Reconnaissance, Scanning, Gaining Access, Maintaining Access, and Covering Tracks. There are still a lot of information that wasn’t included here and I hope it gave you an idea and understand the phases and techniques on how the penetration testers attack a system.

If you have any questions or suggestions, feel free to contact me. 🙂

---

**Subscribe for more!**

To stay up to date with my latest posts and more InfoSec guides, make sure to subscribe to this blog by entering your email address below.

I promise I won’t be spamming your mailbox. Because no one loves spam. AMIRIGHT?

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/more-spam.gif?resize=768%2C576&ssl=1)