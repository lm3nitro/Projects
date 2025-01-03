# Introduction

The SOC received an alert about a quarantined email that was flagged by the email gateway. The email was sent to a user from their friend. Your task is to analyze and review the email to determine if the email is safe to release to to the users inbox or if further action is needed.

Below is a screenshot of the email that was received:

![Pasted image 20250103060132](https://github.com/user-attachments/assets/fd685a85-e9bf-4207-bf90-56b5d48ac1bc)

## Description

A user received an email from a friend outside the organization that includes an attachment (survey), the email security gateway flagged the email. 

## Headers

A python script was used to extract the header information from the eml file:

```
eioc Invitation_survey.eml
```

![Pasted image 20250103061235](https://github.com/user-attachments/assets/7692f534-5bc4-4867-9b37-4473ee95415e)

+ **Date:** Tue, 14 May 2024 23:31:08 +0000
+ **Subject:** You're Invited!
+ **To:** emily.nguyen@glbllogistics.co
+ **From:** Adam Barry <abarry@live.com>
+ **Return-Path:** <abarry@live.com>
+ **Message-ID:** SA1PR14MB737384979FDD1178FD956584C1E32@SA1PR14MB7373.namprd14.prod.outlook.com

## Attachments

First, I used the emldump python script to see what attachments there were in the email. Based on the ouput, the file attachment is a Word Document:

```
emldump Invitation_survey.eml
```

![Pasted image 20250103062248](https://github.com/user-attachments/assets/47b7a1a0-9e6d-43e0-8c64-165bb4cecb38)

I was then able to get more information on the file. In this case, the file was created using Word 2007:

```
emldump Invitation_survey.eml -s 5 -d > AR_Wedding_RSVP.docm
```

![Pasted image 20250103062613](https://github.com/user-attachments/assets/baf8171f-a43f-4fc7-9eb5-78a3237bc214)

This is the hash information found on the attachment:

+ **Filename:** AR_Wedding_RSVP.docm
+ **MD5:** 590d3c98cb5e61ea3e4226639d5623d7
+ **SHA1:** 91091f8e95909e0bc83852eec7cac4c04e1a57c3
+ **SHA256:** 41c3dd4e9f794d53c212398891931760de469321e4c5d04be719d5485ed8f53e

## Extracting the attachment From Email file

Oledump is a tool to analyze OLE files. Many file formats including Microsoft Office uses OLE files for VBA macros. I will be using the tool with the command below:

```
oledump AR_Wedding_RSVP.docm
```

![Pasted image 20250103062843](https://github.com/user-attachments/assets/fb365a83-b390-4bd3-add6-0b79d7d0e014)

Based on the output above, the imbedded macro is present in string 3. Next, I took a closer look at string 3:

```
oledump AR_Wedding_RSVP.docm -s 3
```

![Pasted image 20250103063543](https://github.com/user-attachments/assets/42f7fa59-2250-460d-abed-dc9722910873)

Next, I extracted the the object from the attachment:

```
oledump AR_Wedding_RSVP.docm -s 3 --vbadecompresscorrupt
```

![Pasted image 20250103063907](https://github.com/user-attachments/assets/429bb5b1-af93-4c27-aee5-11982b55cee6)

## Artifact Analysis

+ **URL Analysis:**
This was the link that was referenced within the file:

```hxxps[://]github[.]com/TCWUS/Pastebin-Uploader[.]exe```

Upon researching the link, at the time of this analysis the URL seems to be taken down. The HTTP 404 is a standard response code that indicates the server could not find the requested resource.

![Pasted image 20250103064936](https://github.com/user-attachments/assets/45064478-7115-4cbd-a803-a2278f850e53)

+ **Attachment Analysis:**

I also provided the attachment hash to VirusTotal for analysis:

```https://www.virustotal.com/gui/file/41c3dd4e9f794d53c212398891931760de469321e4c5d04be719d5485ed8f53e```

![Pasted image 20250103061254](https://github.com/user-attachments/assets/9ca4e016-f7a2-484c-aae9-481c84fc8577)

SHA256: 41c3dd4e9f794d53c212398891931760de469321e4c5d04be719d5485ed8f53e

For a dynamic analysis, I used Hybrid Analysis:

```https://www.hybrid-analysis.com/sample/41c3dd4e9f794d53c212398891931760de469321e4c5d04be719d5485ed8f53e```

![Pasted image 20250103065548](https://github.com/user-attachments/assets/23180aa8-d416-49ae-b491-c0c7a2f90f93)

Below is information that was provided by Hybrid Analysis:

**Network Analysis Overview:**

![Pasted image 20250103064207](https://github.com/user-attachments/assets/2803a522-a74c-4bf6-991d-9e32abfac93a)

**Malicious Indicators:**

![Pasted image 20250103070227](https://github.com/user-attachments/assets/d29fdac7-eab5-4ef4-a9a9-e6b7a807ea3d)

**Information on Malicious VBA Code:**

The VBA code defines a private subroutine named "Document_Open" which is automatically executed when the document is opened. 

+ 1. Object Creation: The subroutine begins by creating three objects:
    - `http_obj`: An instance of the "Microsoft.XMLHTTP" object, commonly used for handling HTTP requests.
    - `stream_obj`: An instance of the "ADODB.Stream" object, used for data manipulation and file interaction.
    - `shell_obj`: An instance of the "WScript.Shell" object, enabling interaction with the Windows shell and execution of commands.
+ 2. Variable Initialization: Three variables are then initialized:
    - `URL`: Set to "hxxps[://]github[.]com/TCWUS/Pastebin-Uploader[.]exe", indicating an external URL as the target.
    - `FileName`:  Set to "shost.exe", suggesting the intended name for a file.
    - `RUNCMD`: Set to "shost.exe", indicating an attempt to execute a file named "shost.exe".
+ 3. HTTP Request: The `http_obj` is then used to make a GET request to the specified URL, retrieving the content from the target address.
+ 4. Data Handling: The received data from the HTTP request is then written into the `stream_obj` and saved to a file named "shost.exe" in the current directory.
+ 5. Command Execution: Finally, the code utilizes the `shell_obj` to execute the "shost.exe" file that was downloaded.

## Verdict
After conducting the analysis on the attachment extracted from the email, I was able to extract a document with an embededed macro. Based on the output of both resources (Hybrid Analysis and VirusTotal), the attachment linked in the file is malicious. 

## Defense Actions

1. Conducted a search of the file hash on all the Endpoints to ensure that it was not present and no host was compromised. 
2. Blocked the URL at the proxy level.
