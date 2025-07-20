---
layout: post
title: "How to identify phishing emails"
source: "https://anotsodev.me/how-to-identify-phishing-emails/"
author:
  - "[[anotsodev]]"
date: 2020-04-11
description: "This blog post will help you identify phishing emails and learn what are the tools being used in checking if the sender and links are legitimate. The scenario in the next section will tell you the …"
tags:
  - "clippings"
---
This blog post will help you identify phishing emails and learn what are the tools being used in checking if the sender and links are legitimate.

The scenario in the next section will tell you the example of phishing email attacks and their consequences.

So let’s say you have received an email that is very similar to the services that you are frequently using. For example, from Paypal saying that someone logged in to your account, or from coins.ph (digital wallet used in the Philippines) saying that you have an unclaimed reward because you and your friends are verified.

Since it has gotten your attention and you are indeed convinced that you have a reward waiting to be claimed, you unknowingly ignored the red flags such as the email address of the sender and the inconsistencies of the email format. So you clicked the button and it redirected you to a familiar site but with an unfamiliar URL. Now you entered your credentials and clicked the login button but instead of prompting you to enter the authentication code from your 2FA device, it redirected you to the real site of that service you are using.

Now you are wondering, what happened? Let me tell you…

**You just got phished!**

The fake site where you entered your credentials has already gotten your login information. Your email address, your username, and password. Everything.

So if you are using similar credentials to other services, you are basically *f’d* *up*.

Now, to protect you and help you to be vigilant. Take your time to read this post and familiarize yourself with several characteristics about the emails that you have received that would indicate that these could be phishing emails.

This post will cover the basic approach to identify phishing emails.

Just to give you the overview, here’s the outline of this post.

- Identify phishing emails by Internet Headers analysis and Email Reputation Lookup
- Containment Approaches
- Summary

## Identify phishing emails by Internet Headers analysis and Email Reputation Lookup

In this section, you will learn how to analyze headers and identify the source IP of the sender or the relay server that they have used to send the phishing email.

We’ll be using this tool called [MxToolbox](https://mxtoolbox.com/EmailHeaders.aspx). You may read the brief description from their site to learn more.

> ABOUT EMAIL HEADERS
> 
> This tool will make email headers human readable by parsing them according to RFC 822. Email headers are present on every email you receive via the Internet and can provide valuable diagnostic information like hop delays, anti-spam results and more.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/image.png?fit=1024%2C372&ssl=1)

MxToolbox Email Header Analyzer

So before we use this tool, let’s get first the internet headers from the suspected phishing email. I will be using the internet headers of the email that was sent to my Protonmail address.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/image-1.png?resize=1008%2C808&ssl=1)

Coins.ph phishing email sent to Protonmail

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/image-2.png?fit=1024%2C517&ssl=1)

Example email sent to Gmail

You may view the internet headers of your email on Protonmail by clicking the dropdown button then **View Headers**. On gmail, click the ‘kebab’ (*three*  vertical  *dots*) button then Show original.

After clicking the **View Headers** or **Show Original** button, just copy all the content then paste it to the email header analyzer tool then click the Analyze Header button.

After you submit the internet headers for analysis, you’ll see this page which contains the summary of the analysis.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/image-3.png?fit=1024%2C504&ssl=1)

Header Analysis Summary

On this email, you may notice that the sender used Gmail to send his/her phishing email

Since we know that Gmail IPs are reputable and are not really dangerous, let’s just use another phishing email internet header for the sake of demonstrating the identification of dangerous sender’s source IP.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/image-4.png?resize=1170%2C267&ssl=1)

Email sent from blacklisted/spammer IP

So you may notice that the sender’s IP is **185.234.219.119**. We can copy the IP address to check its reputation using open-source intelligence services such as **[Talos Intelligence](https://talosintelligence.com/)** by Cisco.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/image-5.png?resize=1170%2C768&ssl=1)

IP Reputation Lookup result

By using this tool, you can see details such as the location, reputation, owner, content, and blacklists that may help you to assess how dangerous the sender is.

Now, let’s move back to the MxToolbox analysis result to get more details about the email.

![](https://i0.wp.com/anotsodev.me/wp-content/uploads/2020/04/image-7.png?resize=1170%2C532&ssl=1)

Headers Found

You may notice that the display name of the sender is not consistent compared to his/her email address. So this will hint us that the email is not really legitimate.

For the header name starting with “X-“, these are custom header and proprietary set by relay servers or mail gateways.

Let’s now move on to the next section, which is the containment approaches if you unknowingly entered your credentials to the phishing site.

## Containment Approaches

Let’s say you have entered your credentials to the phishing site and redirected you to the official site, you kept wondering why it did not prompt you to enter 2FA code. You checked again the email and saw that the sender’s details are not consistent, you have realized that you have entered your credentials to a phishing site since you did not notice the URL since you were using your phone when you clicked the link embedded on the email.

So what should you do about this incident?

These are the steps that you **MUST** do to contain and prevent the attacker to abuse your credentials:

1. Install a password manager (1password, Enpass, or Bitwarden) then generate hard to guess password then change your password immediately
2. Log-out all the sessions of your account
3. If you are using similar login details on other services, change their passwords immediately
4. Install virus scanner software then run a full scan in case of malicious executables downloaded on your device
5. Delete the email on your inbox

After doing these steps, you have now ensured that your account is safe again and won’t be abused by the attacker.

## Summary

Just to summarize the topics that I’ve discussed here, we tackled how to spot phishing emails by checking the internet headers and using tools such as **MxToolbox** and **Talos Intelligence** to aid us with our analysis. Lastly, we tackled the necessary steps in securing your account again after being a victim of phishing.

So that’s it! I hope you have learned new technologies and tools by reading this blog post. I will be posting more guides and tutorials again soon whenever I have time.

May the force be with you.

[Featured Image created by katemangostar – www.freepik.com](https://www.freepik.com/free-photos-vectors/abstract)

---
