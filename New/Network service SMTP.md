# SMTP

![Pasted image 20241003085910](https://github.com/user-attachments/assets/12630579-a0bf-4a4b-93c4-9e5b9cc7fa23)

SMTP, or Simple Mail Transfer Protocol, is a protocol used for sending emails across the Internet. It facilitates the transmission of email messages from a sender's mail server to a recipient's mail server. Below is the process that takes place between client and the server:

![Pasted image 20241003090610](https://github.com/user-attachments/assets/8309fba5-8b70-4747-9589-719cd4a3c934)

### Scope:
In this exercise, I will be performing enumeration on a SMTP server to verify ports and possible misconfigurations. With the provided information from the enumeration, I will use Hydra to crack the password of one of the users and use those credentials to SSH into the server. 

### Tools and Technology:
Linux, Metasploit, Telnet, Nmap, Wireshark and SMTP

## Enumeration:

First, I used Nmap to scan against the target host and gather information on ports and services:

```
nmap -sV -sC 10.10.246.91 -nnvvv
```

![Pasted image 20241003092220](https://github.com/user-attachments/assets/3fc3e53b-268e-4b5b-91ea-76d3cccf73d0)

I see that both ports 22 (SSH) and port 25 (SMTP) are open. I then used the nmap NSE script for smtp:

```
nmap --script smtp* -p 25 10.10.246.91 -nnvvv 
```

![Pasted image 20241003093718](https://github.com/user-attachments/assets/eaf98569-4000-4f77-9362-9e3b01f4dc3d)

I can see that there is a vulnerability (CVE-2010-4344). Knowing this, I took a closer look into the vulnerability:

![Pasted image 20241003093831](https://github.com/user-attachments/assets/83a0a6b4-ef49-418e-b832-fd192341d2f1)

## Telnet:

Another way to query the STMP service is using telnet. Using Telnet will indicate whether the specified username exists: a response code of 252 means the user exists, and a 550 code indicates the user is unknown.

![Pasted image 20241003093336](https://github.com/user-attachments/assets/0694813a-c3a7-48c1-8f9a-285d1179e49b)

While performing telnet, I also took at a look at the network traffic to see if it can provide any additional information:

![Pasted image 20241003093435](https://github.com/user-attachments/assets/d5ad241d-29c9-41a7-9ee5-3237c0a63d37)

A look into one of the packets:

![Pasted image 20241003094343](https://github.com/user-attachments/assets/2bdac4b0-dcc5-4fcc-a1d4-86ff6b0b8e71)

## Detecting SMTP Enumeration:

While looking at the network traffic, it's important to also note how this traffic looks and indicators of enumeration that might be occurring. In this case I am performing the enumeration myself, but its important to detect these patterns as they can help in identifying potential security threats early.

Filters:
+ smtp.req.parameter
+ smtp.req.parameter
 
![Pasted image 20241003103244](https://github.com/user-attachments/assets/dfd9b2e5-2f94-4763-962b-7fe4cd7e5e10)

Code messages that include the word "root" are anomalies or indicators of footprinting:
 
![Pasted image 20241003094518](https://github.com/user-attachments/assets/c991c19c-1c62-40b8-ac68-af28f44e239c)

## METASPLOIT:

![Pasted image 20241003091852](https://github.com/user-attachments/assets/24aa3ab5-80eb-42a7-8570-8475cd523df6)

Metasploit is a widely used penetration testing framework that provides tools for developing and executing exploit code against remote targets. For this exercise, I used the SMTP Auxiliary Module (smtp_version module). The smtp_version module, as its name implies, will scan and determine the version of mail servers it encounters.

![Pasted image 20241003095321](https://github.com/user-attachments/assets/bb82bc66-210f-41bb-aa9d-ec18a8fd6bd7)

```
search smtp_version
```

![Pasted image 20241003100626](https://github.com/user-attachments/assets/8166119d-54d9-47dd-952d-adb9a193d756)

```
use auxiliary/scanner/smtp/smtp_version
```
```
show options
set RHOSTS 10.10.246.91
set THREADS 254
run
```

Next, I used the SMTP Enumeration module. This attempt to connect to a given mail server and use a wordlist to enumerate users that are present on the remote system.

```
search smtp_enum
```

![Pasted image 20241003095709](https://github.com/user-attachments/assets/6e5698fd-9ad1-4871-a4b8-98ba3383909c)

```
use auxiliary/scanner/smtp/smtp_enum
show options
set RHOSTS 10.10.246.91
run
```

![Pasted image 20241003101932](https://github.com/user-attachments/assets/4f6fea0c-4014-4042-aafc-00460dea8627)

Below is the password list I used:

```
_apt, administrator, backup, bin, daemon, dnsmasq, games,gnats, irc, landscape, list, lp, lxd, mail, man, messagebus, news, nobody, pollinate, postfix, postmaster, proxy, sshd, sync, sys, syslog, systemd-network, systemd-resolve, systemd-timesync, uucp, uuidd, www-data
```
```
set USER_FILE /usr/share/wordlists/SecLists/Usernames 
```

Below is the information I was was able to gather from the enumeration:

+ User:  administrator
+ SMTP server/Operating System: polosmtp.home; OS: Linux; CPE: cpe:/o:linux:linux_kernel

## Exploiting SMTP:

In order to exploit the SMTP server, I will be using Hydra. Hydra is a popular password-cracking tool that allows users to perform brute-force attacks on various services. 

![Pasted image 20241003103939](https://github.com/user-attachments/assets/bcf3a888-263e-4458-b78e-cc5c3314c47f)

Below is a list of possible usernames:

![Pasted image 20241003105302](https://github.com/user-attachments/assets/ef1880ca-05f0-406b-bf8a-82727dba0465)

I then ran the following command to use the list of the 31 users against 14344391 passwords:

```
hydra -t 16 -L users.txt -P /usr/share/wordlists/rockyou.txt -vV 10.10.246.91  ssh
```

![Pasted image 20241003105819](https://github.com/user-attachments/assets/6a0a0c37-9aa5-41bb-bb88-4a4bf5c4c202)

Once it ran, I was able to find the username `administrator` along with the password `alejandro`. 

![Pasted image 20241003111546](https://github.com/user-attachments/assets/36c2cb35-8279-4e3b-95d8-870edb615094)

## SSH

Now that I had the needed credentials, I logged in to the server as the user:

```
ssh admistrator@10.10.246.91
```

![Pasted image 20241003113140](https://github.com/user-attachments/assets/72e2f246-e283-45e0-a570-68963c4f2c30)

I was able to successfully login to the server!

## Detecting 

As previously stated, it's also important to take note of the traffic that is happening while these enumeration and password cracking techniques are used so that these types of attacks can more easily be detected. Below are indicators of attack and enumeration that I found are important:

1. ERROR_SSH_KEY_EXCHANGE_FAILED
+ code: 103 (0x0067)
+ Key exchange failed

![Pasted image 20241003110641](https://github.com/user-attachments/assets/70e55576-740b-4241-9319-5efcc04bcc3f)

2. ERROR_SSH_AUTHENTICATION_FAILED
+ code: 10 (0x000A)
+ Authentication failed. There could be wrong password or something else

![Pasted image 20241003110612](https://github.com/user-attachments/assets/073c816b-9906-49b1-97af-2c0ae2320cfa)

3. ERROR_SSH_HOST_NOT_ALLOWED_TO_CONNECT
+ code: 101 (0x0065)
+ Connection was rejected by remote host

I also noticed that there were many connections with a very short duration. This can also indicate an anomaly on the server:

![Pasted image 20241003110347](https://github.com/user-attachments/assets/bc73fc40-c769-4121-9211-2c7af6347ee3)

### Summary:

In this exercise, I was able to perform enumeration on a SMTP server. The information gathered from the enumeration provided information including one of the usernames. I then used this information and used **Hydra** to find the complete user credentials. Hydra was able to crack the password, and I was able to login to the server via SSH. Performing these steps also demonstrated the importance of verifying server configurations are correct and to regularly review them to avoid misconfigurations that could expose vulnerabilities. In order to protect against these types of exploitation, it's important to implement strong authentication methods and use TLS encryption to secure email communications. 
