---
layout: post
title: "Poor man’s log collection on Windows"
source: "https://anotsodev.me/wec-and-wef-poor-mans-log-collection-on-windows/"
author:
  - "anotsodev"
date: 2020-03-27
excerpt: "Introduction As your organization grows, a lot more log collectors either commercial or freeware are needed to be installed on your network to get logs from different servers running on Windows or …"
---
## Introduction

As your organization grows, a lot more log collectors either commercial or freeware are needed to be installed on your network to get logs from different servers running on Windows or \*nix.

Let’s say for example this organization called **kyle.biz** has implemented Arcsight SmartConnectors deployed on Linux servers to get event logs on Windows. And based on my experience, setting up the SmartConnectors is tedious and maintaining them is sometimes a pain.

The limitations were also realized since SmartConnector only gets logs on the server static IPs configured during the installation so there is no way we’ll get the logs from other machines that have dynamic IPs.

Upon research, our team has stumbled upon this article from Microsoft’s [documentation page](https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection) and saw that Windows has already built-in log forwarding feature which is powerful and easy to configure!

So without further ado, let’s now configure the Source Initiated WEC and WEF on our servers!

## Prerequisites

**Windows Event Collector**

- **OS:** Windows Server 2016
- **Hostname:** WEC01.internal.kyle.biz
- **RAM:** 16GB
- **CPU:** 4 Cores
- **Storage:**
	- Drive C:\\ – 50GB
	- Drive D:\\ – 60GB (we’ll save our forwarded logs here)
- Joined in a domain.

**Windows Event Source**

- **OS:** Windows Server 2016
- **Hostname:** CONSOLESERVER01.internal.kyle.biz
- Joined in a domain.

## WEC Server Configuration

On the following steps, we’ll be configuring our collector server to receive logs which will be initiated by source computers.

**Step 1:** Open the Event Viewer and click the **Subscription**.

**Step 2:** Right-click the Subscription to create a new Subscription. You can also see other actions on the right pane of the window.

![](https://i1.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-8.png?fit=818%2C490&ssl=1)

**Step 3**: Enter the subscription name and select the subscription type as **Source computer initiated.**

*Before we proceed in adding **computer groups,** let’s first configure the events to collect and advanced settings.*

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-4.png?resize=574%2C559&ssl=1)

**Step 4:** Click the Select Events then click the manual edit/XML tab then copy the XML format query here: [Appendix E – Annotated baseline subscription event query](https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection#appendix-e--annotated-baseline-subscription-event-query)

**Step 5:** Click OK

**Step 6:** Click the Advanced Settings then select Minimize Latency.

**Step 7:** Click OK

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-1.png?resize=581%2C557&ssl=1)

**Step 8:** We’ll now add our domain computers by clicking **Select Computer Groups**.

**Step 9:** Since we are joined in a domain, click Add **Domain Computers** then search for the hostname of the event source. (Ex: CONSOLESERVER01.internal.kyle.biz)

**Step 10:** Click OK

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-7.png?resize=619%2C606&ssl=1)

**Step 11:** Right-click the Forwarded Events then click on properties

**Step 12:** Create folder **Winenvt\\Logs** on drive D:\\ then replace the log path by entering **D:\\Winevt\\Logs\\ForwardedEvents.evtx** or refer to the screenshot below.

![](https://i2.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-6.png?fit=818%2C537&ssl=1)

## Event Source Configuration

On the following steps, we’ll be configuring the source computers to forward logs to our collector server.

**Step 1:** Let’s now configure our event source server machine to point the SubscriptionManagers to our **WEC**.

**Step 2:** Open the Local Group Policy then click the **Event Forwarding** to see the items under that directory.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/03/image.png?fit=818%2C451&ssl=1)

**Step 3:** Click the **Configure target Subscription Manager** and enter on the value

`"Server=[http://WEC01.internal.kyle.biz.org:5985/wsman/SubscriptionManager/WEC,Refresh=60](http://kyle.internal.biz.org:5985/wsman/SubscriptionManager/WEC%2CRefresh=60)"`

**Step 4:** Click OK

![](https://i1.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-3.png?fit=818%2C411&ssl=1)

**Step 5:** Open Command Prompt then enter *`gpudate /force` to apply* the modification in our local group policy.

**Step 6:** Press Enter

![](https://i2.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-2.png?fit=818%2C463&ssl=1)

**Step 7:** Go back to the WEC server then check if the Baseline events already getting logs from our event source machine.

![](https://i1.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-5.png?fit=818%2C518&ssl=1)

**Step 8:** We can already see that the events are already forwarded and ready to use for analysis.

![](https://i1.wp.com/anotsodev.me/wp-content/uploads/2020/03/image-9.png?fit=818%2C534&ssl=1)

So that’s it! I hope you learned something new today and I hope you’ll be using this example to reduce the gap and have greater visibility on your network.

## References:

- [https://docs.microsoft.com/en-us/windows/threat-protection/use-windows-event-forwarding-to-assist-in-instrusion-detection](https://docs.microsoft.com/en-us/windows/threat-protection/use-windows-event-forwarding-to-assist-in-instrusion-detection)
- [https://docs.microsoft.com/en-us/windows/win32/wec/setting-up-a-source-initiated-subscription](https://docs.microsoft.com/en-us/windows/win32/wec/setting-up-a-source-initiated-subscription)
