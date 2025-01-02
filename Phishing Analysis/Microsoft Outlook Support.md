# Introduction
A user received an email claiming that their online access to Outlook had been disabled. Since they were still able to access access the platform, they forwarded the email to the security team to be reviewed. In this exercise, I will analyze the email and determine if it is a genuine request or a phishing attempt.

Below is the email that was received:
![Pasted image 20250102065219](https://github.com/user-attachments/assets/be222a04-94d6-407f-b010-5e0d284bb4ff)

## Description
This email is clamming to be from "Microsoft Fraud Department" informing the user of unusual activities. The email also contains a sense of urgency since it claims that the account has been locked and will be suspended in 24 hours if no action is taken. User must click the verify link provided within the email in order to prevent such actions.

## Headers
Using a python script, I was able to extract the following header information from the eml file:

![Pasted image 20250102070822](https://github.com/user-attachments/assets/b65f6d33-141d-4c53-8664-1c450104ad81)

+ **Date:** Tue, 31 Oct 2023 10:10:04 -0900
+ **Subject:** Your account has been flagged for unusual activity
+ **To:** dderringer@mighty-solutions.net
+ **From:** social201511138@social.helwan.edu.eg
+ **SPF Record:** v=spf1 include:spf.protection.outlook.com -all

![Pasted image 20250102075042](https://github.com/user-attachments/assets/1982c3ea-bd5d-4058-ac7b-416317f18944)

**Return-Path:** social201511138@social.helwan.edu.eg
**Sender IP:** 40.107.22.60
**Resolve Host:** mail-am6eur05on2060.outbound.protection.outlook.com 

![Pasted image 20250102070536](https://github.com/user-attachments/assets/06378f60-0dd2-4476-8314-d456e879279a)

**Message-ID:** JMrByPl2c3HBo8SctKnJ5C5Gp64sPSSWk76p4sjQ@s6

## URLs
The body of the email was encoded using base64, I first decoded the body and then extracted the URLs:

![Pasted image 20250102071725](https://github.com/user-attachments/assets/b2004611-375c-46c6-93ec-482df9d937bb)

hxxps[://]0[.]232[.]205[.]92[.]host[.]secureserver[.]net/lclbluewin08812

## Artifact Analysis

**Sender Analysis:**
The sender is trying to impersonate "Outlook Support Team", however, the sender's domain is not affiliated with Microsoft. The domain that the sender is using is also being flagged as Phishing. It's important to note that SPF, DKIM, and DMARC all show as pass because the sender is authorizing Outlook to send emails in their behalf.

![Pasted image 20250102080223](https://github.com/user-attachments/assets/fe78a878-60a2-49c5-9f7a-cacffe84a0f4)

**URL Analysis:**
After preforming the URL reputation check using URLScan and VirusTotal, it was determined that the URL link within the email was found to be malicious. It looks to re-direct to a phishing website. The site appears to have been taken down, however, given the context of the email it may have led to a potential credential harvesting site.

## Verdict

Due to the sender information being unaffiliated with "Outlook Support Team" this email appears to be an impersonation and spoofing attempt. Additionally, the sender domain, IP and the URL contained within the email was flagged as malicious by Virustotal.
As a result, this email has been determined to be malicious.

![Pasted image 20250102072056](https://github.com/user-attachments/assets/70ee7536-94b2-4784-8c08-327d22623d16)

![Pasted image 20250102073807](https://github.com/user-attachments/assets/621e7ea2-5123-4a01-bd80-6ea8c4b31a77)

![Pasted image 20250102074106](https://github.com/user-attachments/assets/bf2e4e76-5f1e-4034-ac12-96170cdcbc7f)

## Defense Actions

The following steps were taken:
1. Blocked IP and Domain gathered from the email at the Firewall and Proxy server.
2. Blocked the sender email address and Domain on the Email Gateway
3. Verified that no other users received an email based on subject line and sender information
