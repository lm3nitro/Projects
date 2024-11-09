# FTP 

![Pasted image 20240919133005](https://github.com/user-attachments/assets/a0751fbc-f431-40a3-aab8-7570c72c9f0f)

FTP, or File Transfer Protocol, is a standard network protocol used to transfer files between a client and a server over the internet. It allows users to upload, download, and manage files on remote servers easily. FTP can operate in two modes: active and passive, which dictate how the connection is established and how data is transferred.

In active mode let's the server connect to the client, while passive mode allows the client to connect to the server, helping to navigate firewall restrictions.

### Scope:


### Tools and Technology:
Nmap, Wireshark, Linux, Hydra, and FTP

## Enumerating FTP:

To start, I first used namp to scan against the target host and see what information it is able to find:

```
nmap -Pn -sS 10.10.73.116 -p- -T5 -vvvv 
```

While performing the Nmap scan, I also used Wireshark to capture the traffic. Below is a screenshot showing port 21 and why it was identified as being opened, given the behavior seen:

![Pasted image 20240919134047](https://github.com/user-attachments/assets/8c887aa2-2452-4beb-ac6f-6a2ef84e9a0e)

Based on the output of the scan, I see that port 21 is open and the target is using FTP. 

![Pasted image 20240919134313](https://github.com/user-attachments/assets/62c0580c-e92a-4386-af16-1ef9bfdb154e)

Now that port 21 is open and the host is running FTP, I then ran another scan, this time specifically targeting port 21 to get more information:

```
nmap -A -sC 10.10.73.116 -p21 -vvvv 
```

![Pasted image 20240919134513](https://github.com/user-attachments/assets/5d5dc7c8-4375-452b-811b-07d8d7a6e53b)

Information gathered on the target:

+ Service: FTP
+ Port: 21
+ Version: vsftpd 2.0.8
+ OS: Linux
+ ftp-anon: Anonymous FTP login allowed (FTP code 230)

### Enumeration Detection

Below are screenshots taken of the Wireshark capture in order to show how the different information is view in the network traffic and how to identify this behavior. It's important to see how different scans and requests look like in order detect potential reconnaissance activities by attackers. 

TCP traffic:

![Pasted image 20240919135553](https://github.com/user-attachments/assets/29ca822e-99b7-47a6-9257-5369bf7e7157)

ICMP traffic:
 
![Pasted image 20240919135953](https://github.com/user-attachments/assets/74317e5e-faee-4cbc-a530-f7fcb15633b6)

FTP requests:

![Pasted image 20240919134950](https://github.com/user-attachments/assets/302295c9-4e87-4432-a6cb-528f834f8199)

FTP responses:

![Pasted image 20240919135745](https://github.com/user-attachments/assets/8564b1e5-dd23-4633-abdd-da473d5c6fd9)

Payload decoding:

![Pasted image 20240919135658](https://github.com/user-attachments/assets/d3dc2e13-7626-4077-a3a5-0c3df12c530f)


## Login into the FTP server:

Given the information that I was able to find above, I was able to login to the server:

```
ftp 10.10.73.116
```

![Pasted image 20240919142356](https://github.com/user-attachments/assets/e855ceba-0192-4761-9973-2ec2bca56bf6)

The code 230 means that the login was successful. 

> [!TIP]
> Return Codes:
> ![Pasted image 20240919145308](https://github.com/user-attachments/assets/6d06febd-dff7-41f6-a55d-16265d563220)

Listing files and downloading:

![Pasted image 20240919142834](https://github.com/user-attachments/assets/cec12814-61b1-4ae6-a2fb-07af24704a03)

Transfer completed:

![Pasted image 20240919142909](https://github.com/user-attachments/assets/814cf631-344e-43cf-b848-4162ab89037a)

Once the transfer completed, I then went to see the contents of the file:

![Pasted image 20240919151001](https://github.com/user-attachments/assets/c0ba9c2a-3aff-44f5-9759-e611ec0115e3)

I see that this file was signed by a user named Mike. This is important for future use. The output below shows a snip of the network traffic and how it looks after performing the steps above:

![Pasted image 20240919143018](https://github.com/user-attachments/assets/3e308adc-df69-4881-a509-f6bfa7527b99)

## Hydra

![Pasted image 20240919150644](https://github.com/user-attachments/assets/371e9ac5-d187-4ff3-9d79-0d114e150b76)

Hydra is a powerful and flexible password-cracking tool used for performing brute-force attacks on various protocols and services. It supports a wide range of protocols, including HTTP, FTP, SSH, and more. Given that I found the user `Mike` above, I used Hydra to crack the users password. Below is the command used:

```
hydra -t 4 -l mike -P /usr/share/wordlists/rockyou.txt -vV 10.10.73.116 ftp
```

![Pasted image 20240919151353](https://github.com/user-attachments/assets/a112df38-e363-4b65-9d35-a45b4757b55b)

Based on the output, I see that the login is `mike` and the password is `password`. Below is another output of the traffic generated from using Hydra:

![Pasted image 20240919151823](https://github.com/user-attachments/assets/6f8bc2df-2b1c-482f-9b55-8e1718c28946)

### Summary:

In this exercise, I was able to enumerate the FTP server and login which then allowed me to find and download a file providing information into the user Mike. I was then able to utilize **Hyrda** to crack the users password. FTP servers can be targeted by attackers looking to exploit weaknesses, such as unpatched software or weak passwords, which can lead to system compromise. It's important to ensure that servers and services are secure while also being able to identify traffic that may be indicative of enumeration, brute force attacks, and other malicious activity. 


