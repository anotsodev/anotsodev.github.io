---
layout: post
title: "Vulnhub SkyDog 2016 – Catch Me If You Can"
source: "https://anotsodev.me/vulnhub-skydog-2016-catch-me-if-you-can/"
author:
  - "anotsodev"
date: 2017-08-30
excerpt: "The SkyDogConCTF is one of the most enjoyable CTF challenges that I have ever played because it contained a lot of twists and challenges that don’t need advanced exploitation techniques …"
---
The SkyDogConCTF is one of the most enjoyable CTF challenges that I have ever played because it contained a lot of twists and challenges that don’t need advanced exploitation techniques and the tools to solve these challenges were available in Kali Linux.

There were also hints on how to get the flags.

*Flag#1 – “Don’t go Home Frank! There’s a Hex on Your House”*

*Flag#2 – “Obscurity or Security? That is the Question”*

*Flag#3 – “During his Travels Frank has Been Known to Intercept Traffic”*

*Flag#4 – “A Good Agent is Hard to Find”*

*Flag#5 – “The Devil is in the Details – Or is it Dialogue? Either Way, if it’s Simple, Guessable, or Personal it Goes Against Best Practices”*

*Flag#6 – “Where in the World is Frank?”*

*Flag#7 – “Frank Was Caught on Camera Cashing Checks and Yelling – I’m The Fastest Man Alive!”*

*Flag#8 – “Franks Lost His Mind or Maybe it’s His Memory. He’s Locked Himself Inside the Building. Find the Code to Unlock the Door Before He Gets Himself Killed!”*

So after running the netdiscover command to get the IP address of the VM, I fired up Nmap to scan the available ports.

```
nmap -p- -sV -sC -oA nmapscan 192.168.56.102
 Host is up (0.00048s latency).
 Not shown: 65531 filtered ports
 PORT STATE SERVICE VERSION
 22/tcp closed ssh
 80/tcp open http Apache httpd 2.4.18 ((Ubuntu))
 |_http-server-header: Apache/2.4.18 (Ubuntu)
 |_http-title: SkyDog Con CTF 2016 - Catch Me If You Can
 443/tcp open ssl/http Apache httpd 2.4.18 ((Ubuntu))
 |_http-server-header: Apache/2.4.18 (Ubuntu)
 |_http-title: 400 Bad Request
 | ssl-cert: Subject: commonName=Network Solutions EV Server CA 2/organizationName=Network Solutions L.L.C./stateOrProvinceName=VA/countryName=US
 | Not valid before: 2016-09-21T14:51:57
 |_Not valid after: 2017-09-21T14:51:57
 |_ssl-date: TLS randomness does not represent time
 22222/tcp open ssh OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
 | ssh-hostkey:
 | 2048 b6:64:7c:d1:55:46:4e:50:e3:ba:cf:4c:1e:81:f9:db (RSA)
 |_ 256 ef:17:df:cc:db:2e:c5:24:e3:9e:25:16:3d:25:68:35 (ECDSA)
 Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
 # Nmap done at Tue Aug 29 13:41:53 2017 -- 1 IP address (1 host up) scanned in 124.99 seconds
```

There were three open ports:

- port 80
- port 443
- port 22222

The first interesting port that I noticed was the port 22222, so I executed the ssh command.

```
kaipowered@debian:~/Documents/Vulnhub/skydogctf$ ssh 192.168.56.102 -p 22222
 ###############################################################
 # WARNING #
 # FBI - Authorized access only! #
 # Disconnect IMMEDIATELY if you are not an authorized user!!! #
 # All actions Will be monitored and recorded #
 # Flag{53c82eba31f6d416f331de9162ebe997} #
 ###############################################################
 kaipowered@192.168.56.102's password:
```

And there it was. The first flag was shown on the banner message. After getting the first flag, I cracked the given md5 hash here: http://md5online.org/

```
Flag{53c82eba31f6d416f331de9162ebe997}

Decoded MD5 Hash: encrypt
```

I had no idea on how can I use the word ‘encrypt’ to get the next flag, so the next thing I did was open the web browser and open the website hosted on the VM.

![Screenshot from 2017-08-31 01-26-55](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-01-26-55.png?resize=1170%2C586&ssl=1)

And it showed me the homepage of the SkyDogConCTF. I explored the entire website and I only found some interesting and funny testimonials of this CTF challenge.

![Screenshot from 2017-08-31 01-32-09](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-01-32-09.png?resize=814%2C516&ssl=1)

So after viewing all the links on the website, I went back to the homepage and view the page source of the website.

![Screenshot from 2017-08-31 01-34-10](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-01-34-10.png?resize=898%2C680&ssl=1)

And on line 40 of the source caught my attention. So I opened the source of the script on the web browser and it showed me this.

![Screenshot from 2017-08-31 01-36-17](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-01-36-17.png?resize=648%2C127&ssl=1)

The first line of the JS file appeared that it was a hex, and the hint on the Flag #1 made sense quoting “Don’t go Home Frank! There’s a Hex on Your House”. So I went to this site http://www.rapidtables.com/convert/number/hex-to-ascii.htm and converted the hex value to ASCII and it showed me the first flag.

```
flag{7c0132070a0ef71d542663e9dc1f5dee}

Decoded MD5 Hash: nmap
```

I guessed that the decoded hash was a hint or something but I already ran Nmap scan so the next thing I did was visit the https version of the website and view the information of the certificate.

![Screenshot from 2017-08-31 01-45-36](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-01-45-36.png?resize=1170%2C634&ssl=1)

And there it was, the flag#3.

```
flag3{f82366a9ddc064585d54e3f78bde3221}

Decoded MD5 Hash: personnel
```

The next hint was “personnel” and my instinct tells me that it is a directory of the web server. And I was right.

![Screenshot from 2017-08-31 01-52-18](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-01-52-18.png?resize=1170%2C293&ssl=1)

So I checked again the hint for the next flag, which is the Flag #4 and it says that

```
“A Good Agent is Hard to Find"
```

And I can only think of one thing related to the word “Agent” and it was the “User-Agent”. But I had no idea what User-Agent should I use to open the “personnel” directory so after reading the “ACCESS DENIED!!! You Do Not Appear To Be Coming From An FBI Workstation. Preparing Interrogation Room 1. Car Batteries Charging….” message, I opened again the html5.js file and searched for “FBI”. And there, I found two interesting lines in the JS file.

```
/* maindev - 6/7/02 Adding temporary support for IE4 FBI Workstations */
 /* newmaindev - 5/22/16 Last maindev was and idoit and IE4 is still Gold image -@Support doug.perterson@fbi.gov */
```

I searched for the User-Agent of the IE4 and changed my User-Agent to access the “personnel” directory.

```
Mozilla/4.0 (compatible; MSIE 4.0; Windows 98)
```

And I got an access to the “personnel” directory.

![Screenshot from 2017-08-31 02-10-57](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-02-10-57.png?resize=1170%2C591&ssl=1)

```
flag{14e10d570047667f904261e6d08f520f}

Clue = new+flag

Decoded MD5 Hash: evidence

Clue = newevidence
```

So I got the 4th flag and got the clue on how to get the next flag.

And it appeared that I needed to enter the username and password before I can view the content of the newevidence directory.

![Screenshot from 2017-08-31 02-12-54](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-02-12-54.png?resize=1170%2C583&ssl=1)

The first thing I did was to search “Agent Hanratty” on google to see any available information about Agent Hanratty that can be used as a username.

So I found that the first name of Agent Hanratty is Carl and the last name is Hanratty.

Going back to the html5.js file, I noticed that the format of the username was firstname.lastname.

```
doug.perterson@fbi.gov
```

So the username of Agent Hanratty is

```
carl.hanratty
```

I only needed the password to access the newevidence directory and based on the Flag #5 quote

*The Devil is in the Details – Or is it Dialogue? Either Way, if it’s Simple, Guessable, or **Personal** it Goes Against Best Practices*

So I searched for the person that was very close or family member of Agent Hanratty and after searching and trying all the possible passwords, I tried the name “Grace” as the password that I found on this phrase.

*Workaholic Carl Hanratty loses his daughter **Grace** to a divorce.*

And I got an access to the newevidence directory.

![Screenshot from 2017-08-31 02-30-10](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot-from-2017-08-31-02-30-10.png?resize=1170%2C599&ssl=1)

So I downloaded the “Evidence Summary File” and the content of the file was this.

```
flag{117c240d49f54096413dd64280399ea9}

Decoded MD5 Hash: panam
```

It appeared that it was the Flag #5 and the decoded md5 hash was “panam”.

I tried opening “panam” on the web browser hoping that it was also a directory of the web server but there was no “panam” directory on the web server.

There were still two links on the page that I haven’t checked yet. So I clicked the “Possible Location” and there was an image file that can be downloaded, so I downloaded the image file.

![screenshot_from_2017_m2kRw](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/08/screenshot_from_2017_m2krw.jpg?resize=969%2C648&ssl=1)

I tried to steghide the image to see if there were any secret messages embedded in the image file.

```
kaipowered@debian:~/Documents/Vulnhub/skydogctf$ steghide --extract -sf image.jpg
 Enter passphrase: panam # I tried the passphrase panam that i got from the decoded md5 hash of flag #5
 the file "flag.txt" does already exist. overwrite ? (y/n) y
 wrote extracted data to "flag.txt"
```

And I guess I was right and there was a text file named “flag”. I viewed the content of the text file and showed me this.

```
kaipowered@debian:~/Documents/Vulnhub/skydogctf$ cat flag.txt
 flag{d1e5146b171928731385eb7ea38c37b8}
 =ILoveFrance

clue=iheartbrenda
 kaipowered@debian:~/Documents/Vulnhub/skydogctf$
```

There was the flag #6, the decoded md5 hash, and a clue for the next flag. So going back to the hint for the next flag.

*Flag#7 – “Frank Was Caught on Camera Cashing Checks and Yelling – I’m The Fastest Man Alive!”*

The quote “I’m the Fastest Man Alive!” was sounded familiar so the next thing I did was google the quote and search who said that.

*My name is Barry Allen, and I am the fastest man alive.*

It seemed like Barry Allen from The Flash also said the “I’m the Fastest Man Alive!”.

The only last thing that was left to check was the ssh service on port 22222.

```
username: barryallen

password: iheartbrenda
```

With the found username and password, I used them and opened the ssh to login to the VM.

```
kaipowered@debian:~/Documents/Vulnhub/skydogctf$ ssh barryallen@192.168.56.102 -p 22222
 ###############################################################
 # WARNING #
 # FBI - Authorized access only! #
 # Disconnect IMMEDIATELY if you are not an authorized user!!! #
 # All actions Will be monitored and recorded #
 # Flag{53c82eba31f6d416f331de9162ebe997} #
 ###############################################################
 barryallen@192.168.56.102's password:
 Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-38-generic x86_64)

* Documentation: https://help.ubuntu.com
 * Management: https://landscape.canonical.com
 * Support: https://ubuntu.com/advantage

14 packages can be updated.
 7 updates are security updates.

Last login: Tue Aug 29 07:19:16 2017 from 192.168.56.1

barryallen@skydogconctf2016:~$
```

Commands

```
barryallen@skydogconctf2016:~$ ls
 flag.txt security-system.data
```

There were two files present in the user directory of barry allen.

- flag.txt
- security-system.data

The flag #7 and a zip file.

Here are the commands on how I got the Flag #8 and thanks to this walkthrough http://evilcsec.com/skydogcon-ctf-catch-me-if-you-can-walkthrough/ to get the final flag. I needed to see this walkthrough to get to know what tools to use to analyze the data file to get the final flag.

Hint for the final flag.

Flag#8 – “Franks Lost His Mind or Maybe it’s His Memory. He’s Locked Himself Inside the Building. Find the Code to Unlock the Door Before He Gets Himself Killed!”

Commands

```
barryallen@skydogconctf2016:~$ cat flag.txt
 flag{bd2f6a1d5242c962a05619c56fa47ba6} #decoded md5 hash: theflash
 barryallen@skydogconctf2016:~$

barryallen@skydogconctf2016:~$ file security-system.data
 security-system.data: Zip archive data, at least v2.0 to extract

barryallen@skydogconctf2016:~$ unzip security-system.data
 Archive: security-system.data
 replace security-system.data? [y]es, [n]o, [A]ll, [N]one, [r]ename: r
 new name: data
 inflating: data

barryallen@skydogconctf2016:~$ ls
 data flag.txt security-system.data
```

#### Copied the extracted file from barryallen to local machine

```
kaipowered@debian:~/Documents/Vulnhub/skydogctf$ scp -P 22222 barryallen@192.168.56.102:/home/barryallen/data ~/Documents/Vulnhub/skydogctf
 ###############################################################
 # WARNING #
 # FBI - Authorized access only! #
 # Disconnect IMMEDIATELY if you are not an authorized user!!! #
 # All actions Will be monitored and recorded #
 # Flag{53c82eba31f6d416f331de9162ebe997} #
 ###############################################################
 barryallen@192.168.56.102's password:
 data 100% 1024MB 40.1MB/s 00:25
 kaipowered@debian:~/Documents/Vulnhub/skydogctf$

kaipowered@debian:~/Documents/Vulnhub/skydogctf$ volatility imageinfo -f data
 Volatility Foundation Volatility Framework 2.6
 INFO : volatility.debug : Determining profile based on KDBG search...
 Suggested Profile(s) : WinXPSP2x86, WinXPSP3x86 (Instantiated with WinXPSP2x86)
 AS Layer1 : IA32PagedMemoryPae (Kernel AS)
 AS Layer2 : FileAddressSpace (/home/kaipowered/Documents/Vulnhub/skydogctf/tmp/data)
 PAE type : PAE
 DTB : 0x33e000L
 KDBG : 0x80545b60L
 Number of Processors : 1
 Image Type (Service Pack) : 3
 KPCR for CPU 0 : 0xffdff000L
 KUSER_SHARED_DATA : 0xffdf0000L
 Image date and time : 2016-10-10 22:00:50 UTC+0000
 Image local date and time : 2016-10-10 18:00:50 -0400

kaipowered@debian:~/Documents/Vulnhub/skydogctf$ volatility --profile=WinXPSP2x86 -f data filescan > datafile
 Volatility Foundation Volatility Framework 2.6

kaipowered@debian:~/Documents/Vulnhub/skydogctf$ cat datafile | grep flag
 kaipowered@debian:~/Documents/Vulnhub/skydogctf$ cat datafile | grep txt
 0x0000000005e612f8 1 0 -W-r-- \Device\HarddiskVolume1\Documents and Settings\test\Desktop\code.txt
 0x000000000629fc08 1 0 R--rw- \Device\HarddiskVolume1\Program Files\VMware\VMware Tools\vmacthlp.txt
 0x00000000062c4620 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\adobephotoshopcs3.txt
 0x00000000062c4be8 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\adobeflashcs3.txt
 0x00000000062e04b0 1 0 R--r-d \Device\HarddiskVolume1\Documents and Settings\test\Recent\code.txt.lnk
 0x00000000063b4428 1 0 R--r-- \Device\HarddiskVolume1\System Volume Information\_restore{FA371F61-4781-4A7F-99F2-B979D68F9988}\drivetable.txt
 0x0000000006503e60 4 2 -W-rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware VGAuth\logfile.txt.0
 0x000000000663d4c0 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\win7gadgets.txt
 0x000000000663d6b8 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\vmwarefilters.txt
 0x000000000663d8b0 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\visualstudio2005.txt
 0x000000000663daa8 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\vistasidebar.txt
 0x000000000663dca0 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\microsoftoffice.txt
 0x000000000663de98 1 0 R--rw- \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\Unity Filters\googledesktop.txt
 0x000000000663f970 1 0 R--rwd \Device\HarddiskVolume1\Documents and Settings\All Users\Application Data\VMware\VMware Tools\manifest.txt
 0x0000000006640bc8 1 0 R--rwd \Device\HarddiskVolume1\Documents and Settings\test\Desktop\code.txt

kaipowered@debian:~/Documents/Vulnhub/skydogctf$ volatility --profile=WinXPSP2x86 -f data cmdscan
 Volatility Foundation Volatility Framework 2.6
 **************************************************
 CommandProcess: csrss.exe Pid: 560
 CommandHistory: 0x10186f8 Application: cmd.exe Flags: Allocated, Reset
 CommandCount: 2 LastAdded: 1 LastDisplayed: 1
 FirstCommand: 0 CommandCountMax: 50
 ProcessHandle: 0x2d4
 Cmd #0 @ 0x1024400: cd Desktop
 Cmd #1 @ 0x4f2660: echo 66 6c 61 67 7b 38 34 31 64 64 33 64 62 32 39 62 30 66 62 62 64 38 39 63 37 62 35 62 65 37 36 38 63 64 63 38 31 7d > code.txt
 kaipowered@debian:~/Documents/Vulnhub/skydogctf$
```

Convert the hex to ascii.

http://www.rapidtables.com/convert/number/hex-to-ascii.htm

```
66 6c 61 67 7b 38 34 31 64 64 33 64 62 32 39 62 30 66 62 62 64 38 39 63 37 62 35 62 65 37 36 38 63 64 63 38 31 7d
```

Result: flag{841dd3db29b0fbbd89c7b5be768cdc81}

Decoded MD5 Hash: Two little mice

After I got the last flag and the decoded md5 hash, I googled the “Two little mice” and found the quote by Frank Abagnale Sr.

*[Frank Abagnale Sr.](https://www.imdb.com/name/nm0000686/?ref_=tt_trv_qu): Two little mice fell in a bucket of cream. The first mouse quickly gave up and drowned. The second mouse, wouldn’t quit. He struggled so hard that eventually he churned that cream into butter and crawled out. Gentlemen, as of this moment, I am that second mouse.*

The quote of Frank made me goosebumps and I was really satisfied after I finished this CTF challenge. It was quite challenging and I really had fun!