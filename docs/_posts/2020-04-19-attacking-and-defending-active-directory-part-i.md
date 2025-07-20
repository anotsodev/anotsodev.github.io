---
layout: post
title: "Attacking and Defending Active Directory Part I"
source: "https://anotsodev.me/attacking-and-defending-active-directory-part-i/"
author:
  - "anotsodev"
date: 2020-04-19
excerpt: "Introduction Microsoft Active Directory is one of the most interesting services to attack since we can gather a lot of information just by checking if the server is misconfigured which enables…"
---
## Introduction

[Microsoft Active Directory](https://en.wikipedia.org/wiki/Active_Directory) is one of the most interesting services to attack since we can gather a lot of information just by checking if the server is misconfigured which enables us to enumerate treasure trove of information like Domain Users, Administrative Shares, Password Policies and many more.

In this series, I will be separating this into three parts. The first part will include the introduction of Active Directory and its components.

The second part will include different types of attacks against Active Directory and the demonstration on how to execute these attacks.

And lastly, we’ll be covering different approaches in defending and reducing attack surfaces of Active Directory.

## What is Active Directory?

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/active-directory-logo.png?resize=600%2C283&ssl=1)

Active Directory or AD is a centralized Windows OS directory service that stores information about objects on the network and automates the management of user data, security, and distributed resources.

## Active Directory Components

![](https://i2.wp.com/anotsodev.me/wp-content/uploads/2020/04/Components.png?fit=1024%2C683&ssl=1)

Just to give you an overview of the different components of Active Directory, the Active Directory consists of:

- **Domains**
- **Forests**
- **Sites**
- **Domain Controllers**
- **Organizational Units (OU)**

Each of these components has their own purpose that enables Active Directory to function within an organization.

I will just summarize the meanings and functions of these components but you can read more about Active Directory on the [official documentation](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) of Microsoft.

### Domains

Active Directory domains are where the users and computers are located. Each domain holds a database that contains the information of an object’s identity.

For example, I have an AD domain called internal.anotsodev.org. This domain contains all the information of users and computers joined in this domain.

### Forests

Active Directory forests are the topmost logical containers of the hierarchy. The illustration below shows the organizational domain forest model of **internal.anotsodev.org**.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/AD-Forest.png?resize=633%2C412&ssl=1)

### Sites

Active Directory Domain Sites enables the users to authenticate with the nearest domain controller and provides domain preference either administrative or least privilege to the current logged in user.

Sites are organized by one or more IP subnets based on the geographic location of the domain users joined to a domain.

### Domain Controllers

Active Directory Domain Controllers handles authentication requests and verifies users if they have access to domain resources. Domain Users’ preference and permissions are defined via group policy set to the Domain Controller.

### Organizational Unit (OU)

An organizational unit (OU) is a subdivision within an Active Directory into which you can place users, groups, computers, and other organizational units. You can create organizational units to mirror your organization’s functional or business structure. Each domain can implement its own organizational unit hierarchy. If your organization contains several domains, you can create organizational unit structures in each domain that are independent of the structures in the other domains.

-

So that ends the first part of the “Attacking and Defending Active Directory” series.

I will be posting the second part of this series which is the actual attacks that are being used against active directories next week.

Stay tuned and be safe!

**References:**

- [https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [https://www.varonis.com/blog/active-directory-forest/](https://www.varonis.com/blog/active-directory-forest/)
- [https://kb.iu.edu/d/atvu](https://kb.iu.edu/d/atvu)
- [https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)

