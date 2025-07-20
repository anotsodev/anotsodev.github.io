---
layout: post
title: "Networking Concepts and Protocols"
source: "https://anotsodev.me/networking-concepts-and-protocols/"
author:
  - "[[anotsodev]]"
date: 2020-03-28
description: "This blog post contains the Networking Concepts and Protocols course that can be enrolled in Pluralsight. Feel free to skim this outline to get a high-level overview of the topics that were discuss…"
tags:
  - "clippings"
---
This blog post contains the Networking Concepts and Protocols course that can be enrolled in Pluralsight. Feel free to skim this outline to get a high-level overview of the topics that were discussed on the course.

## Introduction

- What is networking?
- Concepts
- Modeling network communication

## Data Networking

- Data moving from a device to another device

## Understanding Data Networking

- protocols to protocols

### Modeling Systems

- - Concept
	- Language
	- Link
	- Physical

### The OSI Model

- Open Systems Interconnect

#### Physical Layer (Layer 1)

- Physical Devices
- Cables
	- Twisted Pair
	- CoAx
	- Fiber Optics
	- Copper

#### Data Link Layer (Layer 2)

- Ethernet
- DOCSIS-3 (ISP to Internet)
- Move data from one device to another device

#### Network Layer (Layer 3)

- IP Addressing
- IP Routing

#### Transport Layer (Layer 4)

- Session between client and server
- TCP – Transmission Control Protocol
- UDP – User Diagram Protocol

#### Session Layer (Layer 5)

#### Presentation Layer (Layer 6)

- Decoding ASCII
- EBCDIC (IBM Encoding)

#### Application Layer (Layer 7)

- HTTP
- HTTPS

## Protocols and Port Numbers

#### Application Layer Protocols (Layer 7)

- Transferring Data
- Hypertext Transfer Protocols
	- HTTP (Layer 4 port: 80)
	- HTTPS (Layer 4 port: 443)
- File Transfer Protocols
	- FTP (Port 20)
	- sFTP (Port 22)
		- FTP over SSH
	- TrivialFTP (Port 69)
		- Used to transfer files without authentication
	- SMB (Port 445)
- Email Protocols
	- SMTP (Port 25/465/587)
		- Used for outbound emails
	- POP3 (Port 110/995) /IMAP (Port 143/993)
		- Used for inbound emails
- Authentication Protocols
	- LDAP (Port 389)
	- LDAPs (Port 636)
- Network Services
	- DHCP
	- DNS
	- NTP
- Network Management
	- Telnet
	- SSH
	- SNMP
	- RDP
- Audio/Video Protocols
	- H.323
	- 1720
	- SIP

#### Transport Layer Protocols (Layer 4)

- TCP
	- Transmission Control Protocol
	- Uses 3-way handshake to establish session with other devices
		- SYN – SYN/ACK – ACK
	- Uses 4-way disconnect to end established session with other devices
		- FIN – FIN/ACK – FIN – FIN/ACK
		- RST
			- Immediately end established session
- UDP
	- User Diagram Protocol
	- Only sent packets without any sequence
	- Does not establish session

#### Transport Layer Addressing

- Port Numbers
- - Well Known – 1024 – 49151
- Client Port Numbers (Temporary)
	- 49152 – 61535

## Binary and Hexadecimal

- Base 2 (0 – 1)
- Base 10 (0 – 9)
- Base 16 (0 – 15) (0 – F)

## Introduction to IP Addressing

- Classful
- Classless
- What is an IP address?
	- \[ 192.168.1 \] – Network Portion \[.1 \] – Host Portion
- Classless Addressing
	- Subnet Mask
		- Network portion where all bits are filled
- Classful Addressing
	- Unicast
		- Class A
		- Class B
		- Class C
	- Multicast
		- Class D
- Address Types
	- IP Address Types
		- Network Address
			- Identifier for a group of devices
		- Broadcast Address
			- Identifier for all devices on a network
		- Host Address
			- Identifies unique device on a network
	- Network Address
	- CIDR Notation
		- Example: 10.1.1.0/24
- Subnetting Networks
	- Variable Length Subnet Masking
- Introduction to IPV6
	- 128 bits long
	- 32 nibbles
	- 8 hextets
	- Network Portion = 64 bits
	- Interface Identifier = 64 bits
	- Leading 0’s =::
	- 2^64 available hosts
	- Additional Details
		- Dual Stack
		- Unicast Address
		- fe80 = Link Local Address
	- IPV6 Address Acquisition
		- SLAAC
			- Windows
				- Random 64 bit Interface Identifier
			- Unix
				- Modified EUI-64
					- Get MAC Address
					- Break into half
					- add ff:fe in the middle
					- flip 7th bit on the first 8 bits
		- Neighbor Advertisement
	- IPV6 Tunneling
		- Local router creates a tunnel to the IPv6 router to acquire IPv6 address to access resources.

## Ethernet and Switching

- Layer 2 allows traffic between devices
- Carrier Sense Multiple Access with Collision Detection (CSMA/CD)
	- Collision Domain
- Duplex and speed
	- Half Duplex – one device communicates at a time
		- Walkietalkie
	- Full Duplex – simultaneous communication between devices
		- Telephone
- Modern Collision Domain
- Ethernet Speed
	- GigabitEthernet speeds require full duplex
- Ethernet II Frame
	- Destination MAC Address (48 bits)
	- Source MAC Address (48 bits)
	- Type (16 bits)
	- Data (1500 bytes)
	- FCS (32 bits)

## Network Topologies

- BUS
- Ring
- Start

## Switch

- MAC Address Table
- Broadcast
	- Layer 2 Broadcast address
		- Destination MAC address is all FFF…
	- Broadcast domain – group of devices on a local area network
- Features
	- Broadcast storms prevention
	- VLANs
	- Mirroring switch ports
- Connecting Switches
	- Broadcast Messages
	- Broadcast Storm
	- Spanning Tree Protocols
		- Shuts down redundancy
- VLANs (Broadcast Domain)
	- Trunk Link – connects switches that are using VLNS
	- Adds additional details pointing to destination VLAN
	- Also called as Tagged Ports
	- Trunklinks/Access links
- Switch Port Mirroring
	- Collects traffic to analyze problems within network
	- Mirror the traffic of any port that we want

## Power over Ethernet (POE)

- Gives power to connected devices

## IP Routing

- Routers
	- 2 Interfaces to operate
		- Has unique IP Network
	- IP Packet
![IP Packet](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/03/ip-packet.png?resize=499%2C283&ssl=1)
- - ARP (Layer 2 Protocol)
		- Retrieves layer 2 address using layer 3 address
		- Device maintains an ARP cache
		- ARP cache will age out entries
		- Bridge between L2 and L3
	- Default Gateway (Router)
		- ARP Default gateway
		- All router must be configured to forward traffic to the right gateway
		- IP segments of the destination addresses must be configured to the gateway
		- Every time a packet traversed gateway, its TTL will be decreased by 1
		- Static Routing
		- Dynamic outing
			- Routing Protocols
				- RIP
				- EIGRP
				- OSPF
				- BGP
	- Tools
		- tracert

## Network Services

- Network Topologies
	- LAN/WLAN
	- WAN
		- CAN
		- MAN
	- SAN
	- PAN

## Network Address Translation

- To communicate with the Internet
- What it does
	- Temporarily change the private source IP to a public source IP address
- Port Address Translation

## Port Forwarding

- Socket – IP:PORT
	- 192.168.1.1:1337

## Access Control Lists

- Allow/Deny destination or source IP addresses

## DHCP

- Client will send discover message
- DHCP Offer
- DHCP Binding
- IP Helper Address

## DNS Hierarchy

- Uniform Resource Locator (URL)
	- TLD (.com,.net,.org)
	- SLD (google.com, trendmicro.com)
	- Third Level domain (www)
	- Subdomain (www.xxx.trendmicro.com)
	- Reverse DNS Lookup
		- DNS Record Types
			- A – IPv4 Record
			- AAAA – IPv6 Record
			- CNAME – Canonical Name Record (Alias)
			- MX – Mail Exchange Record
			- NS – Identifies Authoritative Name Server
			- PTR – Pointer Record
			- SRV – Service Record
			- TXT – miscellaneous use

## Internal vs External DNS

- Internal domain name system zone
- External domain name system zone
	- Uses Internet’s DNS

**Course Reference:**

[Pluralsight – Networking Concepts and Protocols](https://app.pluralsight.com/library/courses/comptia-network-plus-networking-concepts/)

---
