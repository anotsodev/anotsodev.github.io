---
layout: post
title: "Vulnhub Tr0ll 1"
source: "https://anotsodev.me/vulnhub-tr0ll-1/"
author:
  - "anotsodev"
date: 2017-08-25
description: "Started with the Nmap Scan # Nmap 7.40 scan initiated Thu Aug 3 20:58:47 2017 as: nmap -sC -sV -oA nmap 192.168.8.102 Nmap scan report for 192.168.8.102 Host is up (0.0035s latency). Not shown: 997…"
---
Started with the Nmap Scan

```
# Nmap 7.40 scan initiated Thu Aug  3 20:58:47 2017 as: nmap -sC -sV -oA nmap 192.168.8.102
Nmap scan report for 192.168.8.102
Host is up (0.0035s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.2
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxrwxrwx    1 1000     0            8068 Aug 10  2014 lol.pcap [NSE: writeable]
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 d6:18:d9:ef:75:d3:1c:29:be:14:b5:2b:18:54:a9:c0 (DSA)
|   2048 ee:8c:64:87:44:39:53:8c:24:fe:9d:39:a9:ad:ea:db (RSA)
|_  256 0e:66:e6:50:cf:56:3b:9c:67:8b:5f:56:ca:ae:6b:f4 (ECDSA)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/secret
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Aug  3 20:59:07 2017 -- 1 IP address (1 host up) scanned in 19.95 seconds
```
- port 21
- port 22
- port 80

![screenshot-from-2017-08-07-14-59-21-e1503681730435.png](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-07-14-59-21-e1503681890612.png?resize=734%2C585&ssl=1)

![screenshot20from202017-08-072015-02-06.png](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot20from202017-08-072015-02-06-e1503681876965.png?resize=656%2C296&ssl=1)

There is a directory in the web server named secret. So I opened the /secret directory and it showed me this.

![screenshot20from202017-08-072015-03-20.png](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot20from202017-08-072015-03-20-e1503682119899.png?resize=745%2C650&ssl=1)

And yes, it made me mad after seeing this.

Continuing my enumeration, I saw that the FTP server allows anonymous login, so I fired up my Filezilla and browse the FTP server. There is a file there named lol.pcap. Pcap is a file that contains the captured network traffics from network analyzers such as Wireshark and some other tools. So to open the pcap file, I launched Wireshark and read the pcap file. It showed me this.

![screenshot20from202017-08-032021-37-00-e1503682196980.png](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot20from202017-08-032021-37-00-e1503710304210.png?resize=1170%2C610&ssl=1)

I noticed the “sup3rs3cr3tdirlol” and tried to open it and voila.

![screenshot20from202017-08-072015-13-36.png](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot20from202017-08-072015-13-36-e1503682263987.png?resize=548%2C388&ssl=1)

I downloaded the file named roflmao then run the file command to determine the filetype of the downloaded file. It shows that it is an ELF 32-bit LSB executable.

```
kaipowered@debian:~/Documents/Vulnhub/tr0ll$ file roflmao 
roflmao: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.24, BuildID[sha1]=5e14420eaa59e599c2f508490483d959f3d2cf4f, not stripped
```

So the next thing I did was to run the gdb then run the executable file.

```
GNU gdb (Debian 7.12-6) 7.12.0.20161007-git
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from roflmao...(no debugging symbols found)...done.
(gdb) run
Starting program: /home/kaipowered/Documents/Vulnhub/tr0ll/roflmao 
Find address 0x0856BF to proceed[Inferior 1 (process 10058) exited with code 040]
(gdb)
```

The output showed me this

```
Find address 0x0856BF to proceed[Inferior 1 (process 10058) exited with code 040]
```

I tried to find the said address but I wasn’t able to find it in the memory, so I tried opening it on the web browser and it appears that it is a directory on the web server (luck I guess).

![screenshot20from202017-08-072015-24-34](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot20from202017-08-072015-24-34-e1503682433212.png?resize=659%2C390&ssl=1)

```
hydra -L which_one_lol.txt -P Pass.txt 192.168.8.102
```

At first, the usernames and passwords combination did not work, so it was so frustrating.

```
hydra -L which_one_lol.txt -p Pass.txt 192.168.8.102
```

But it appears that the real password is the “Pass.txt”, so I just changed the option from -P to -p, meaning, with the given password, try the usernames from which\_one\_lol.txt to match the correct combinations with hydra.

After bruteforcing, I was able to get the username and password of the machine.

```
username: overflow
password: Pass.txt
```

Commands Used

```
kaipowered@debian:~/Documents/Vulnhub/tr0ll$ ssh overflow@192.168.8.102
overflow@192.168.8.102's password: 
Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-32-generic i686)

 * Documentation:  https://help.ubuntu.com/

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Sun Aug  6 23:35:25 2017 from 192.168.8.2
Could not chdir to home directory /home/overflow: No such file or directory
$
```

https://www.exploit-db.com/exploits/37292/

And after successfully exploited the tr0ll vm, I was able to get and read the Proof.txt that can be found in /root directory.

— FIN —

Solving this challenge was quite frustrating at first because, you know, the tr0ll is trolling you, but most of the time, when I found something interesting and it just appeared that I have been tr0lled, I just found myself grinning while clenching my fists, but of course I enjoyed solving this challenge.