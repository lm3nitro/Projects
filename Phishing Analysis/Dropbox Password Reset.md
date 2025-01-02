# Introduction

A user recently had trouble signing into their Dropbox account after trying to access it for the first time in months and reached out to their manager for assistance. The next day, the user received an email that claims a password change request was made for her Dropbox account. The email includes a link for resetting their password, but they are unsure if the request is legitimate.

Below is the email that was recieved:

![Pasted image 20250102133204](https://github.com/user-attachments/assets/d63d0d65-e33b-483b-a99f-9c4433e7469d)

## Description
User received an email to reset Dropbox password. Email contains a link to reset password. Analysis needs to be performed to determine if the email is legitimate or not.

## Headers

I ran a python script to extract the header information fond within the eml file:

![Pasted image 20250102125209](https://github.com/user-attachments/assets/0455a52a-9b07-44ee-a8f3-ac5329ef3a54)

+ **Date:** Sun, 12 May 2024 04:10:52 +0000
+ **Subject:** Reset your Dropbox password
+ **To:** emily.nguyen@glbllogistics.co
+ **From:**  no-reply@dropbox.com
+ **Return-Path:** 0101018f6aff12b2-5bcaa145-861b-45da-a06e-b5c1ee3ca941-000000@email.dropbox.com
+ **Sender IP:**  54.240.60.143
+ **Resolve Host:**  a60-143.smtp-out.us-west-2.amazonses.com 

![Pasted image 20250102132816](https://github.com/user-attachments/assets/2404088e-5edc-4f23-9514-da7e3777cfca)

![Pasted image 20250102132649](https://github.com/user-attachments/assets/88e29ef0-0b6d-471e-bd91-ac036331d747)

+ **SPF:** v=spf1 include:amazonses.com ~all
+ **Message-ID:** 0101018f6aff12b2-5bcaa145-861b-45da-a06e-b5c1ee3ca941-000000@us-west-2.amazonses.com

## URLs

The email was encoded in quoted printable. Once decoded, I extracted the following URLs that were contained within the email:

hxxps[://]www[.]dropbox[.]com/l/ABCIzswwTTJ9--CxR05fYXX35pPA-Y0m3PY/forgot_finish
hxxps[://]www[.]dropbox[.]com/l/AADQ493u2QLcZrv2kCW6C6i0Ac-mR0hUXxU/help/365
hxxp[://]www[.]w3[.]org/TR/REC-html40/loose[.]dtd
hxxp[://]www[.]w3[.]org/1999/xhtml
hxxp[://]fonts[.]googleapis[.]com/css?family=Open+Sans');
hxxps[://]www[.]dropbox[.]com/l/AADiZXaA7dm2EyafvAILlHJAzwU3D55FQwg/forgot_finish
hxxps[://]www[.]dropbox[.]com/l/AAAN2hEjsnK4UD1fkJxpXCS15vzTRW64Tjc/help/365
hxxps[://]www[.]dropbox[.]com/l/AACK_ihQPkvHRxrdwgoAKiptOg-dtAhzX2Y

## Artifact Analysis

+ **Sender Analysis:**
Email domain from sender is showing from Dropbox. The return path and the IP address all show to be from Dropbox.
+ **URL Analysis:**
Upon hovering over the link included in the email, the URL appears to be associated with Dropbox. Upon performing the reputation check on the URL, the link in the email was found to be clean.

## Verdict
The URL reputation from the sender from both Cisco Talos and Virus total both show as clean and affiliated with Drobox. This email appears to be a legitimate request to reset password.

![Pasted image 20250102132708](https://github.com/user-attachments/assets/37a9f5d2-d08b-49dc-b7ad-42774d6dd519)

![Pasted image 20250102132716](https://github.com/user-attachments/assets/dd800f3c-b13c-4de6-ae95-942439682206)

## Defense Actions
No action needed, this is a genuine email.
