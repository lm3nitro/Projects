# Introduction

The following is an analysis of an email that was received. I have been tasked with determining whether this email is legitimate or potentially a phishing attempt. Below is a screenshot of the email as it appeared in the inbox for reference:

![Pasted image 20250101093852](https://github.com/user-attachments/assets/6a015fd4-7222-4478-b26f-74e675e7cea6)

Below is the complete report and email analysis:
____________________________________________________________________________________________________________________________________
## Description

The user received an email claiming to be from "Amazon Prime Support" requesting the user to click on link to verify their account. There is a sense of urgency that account will be suspended if no action is taken before said date, "September 17, 2022".

## Email Headers

I used a python script to parse through the email and extract the important header information:

```
eioc amazon_prime.eml
```

![Pasted image 20250101095623](https://github.com/user-attachments/assets/a1adca54-266c-41b9-a8da-3815c0038e16)

+ **Date:** Fri, 16 Sep 2022 19:20:13 +0000
+ **Subject:** Re: Reminder: [Activity Report] Your account is sign in on a new device. Friday, September 16, 2022 #[636274168]
+ **To:** asmith@hotmail.com
+ **From:** cafepress@mail.cafepress.com
+ **Return-Path:** msprvs1=XjGVrJidPKaSN=bounces-098020-32419@tbh51blx.imdreampores.ovh
+ **Sender IP:** 209.85.221.104

To get the **'Resolve Host**', I searched for the sender IP information:

https://whois.domaintools.com/209.85.221.104

![Pasted image 20250101100741](https://github.com/user-attachments/assets/c0d896aa-20b6-4534-92e6-e6ff6ece3ebb)

+ **Resolve Host:** mail-wr1-f104.google.com 
+ **Message-ID**: <Ti6MiLxTCWMiAH1sP7JxbGrEJIqKsD3Nv4CKIa8Mwrs@mail-pf1-f420.googlegroups.com>

## URLs

In the original email, there is a button that it is requesting the recipient to click in order to go into their account. I hovered over the the link button in order to view the the link address:

![Pasted image 20250101094616](https://github.com/user-attachments/assets/e3bbe14a-eaa5-44b1-a0e6-939591e3ad4f)

Using the same script as above, I used grep to search for the keyword 'cabinet' as mentioned in the link from the screenshot above:

![Pasted image 20250101101122](https://github.com/user-attachments/assets/16abbd8d-09e8-49a6-9f3a-cd8840e67b0a)

This the the defanged URL:

hxxps[://]cabinetlekagni[.]com/

## Artifact Analysis

**Sender Analysis:**
The sender is claiming to be from "Amazon Prime Support", however, the address the email originated from is from an unrelated domain. The information gathered in the header indicates that the email originated from "Google.com" and used OVH Cloud hosting, both of which are not affiliated with Amazon.

**URL Analysis:**
After preforming the URL reputation check using URLScan and VirusTotal, the URL within the action button of the email was found to be malicious. The provided re-directs to a phishig website that is asking for user login credential, potentially preforming credential harvesting.

Re-direct URL: hxxps[://]sbffidserv[.]sviluppo[.]host/s/Dose/signin[.]php

![Pasted image 20250101101815](https://github.com/user-attachments/assets/85eb88f4-0ae6-4a5b-a723-9dbe75e00fe4)

## Verdict
Due to the sender information being unaffiliated with "Amazon Prime", this email appears to be an impersonation and spoofing attempt. Additionally, the URL within the email was flagged as malicious by URLScan, VirusTotal and PhishTank.

![Pasted image 20250101111309](https://github.com/user-attachments/assets/6ec413e0-3ad1-44ee-9477-e0413126fa6e)

![Pasted image 20250101101550](https://github.com/user-attachments/assets/0af6ae12-5413-4500-8b5e-a777f052a149)

![Pasted image 20250101101801](https://github.com/user-attachments/assets/63564431-ba6c-485b-8e01-2be37398a71c)

As a result, this email has been determined to be malicious.

## Defense Actions
After performing a check in the email gateway, no other users received an email from this sender or with this subject line. To prevent the malicious sender from sending any other emails, I have blocked the "cafepress@mail.cafepress.com" email address and blocked the URL on the Fortinet firewall.
![Screenshot 2025-01-01 at 11 08 04 AM](https://github.com/user-attachments/assets/49b4a1ce-0adc-4053-8e4e-3fb23518f8ea)
![Screenshot 2025-01-01 at 11 14 46 AM](https://github.com/user-attachments/assets/785c8cf2-4fe1-4a15-8cd5-ee4a4fa92f79)
