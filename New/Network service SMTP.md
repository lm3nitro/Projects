


![Pasted image 20241003085910](https://github.com/user-attachments/assets/12630579-a0bf-4a4b-93c4-9e5b9cc7fa23)


SMTP stands for "Simple Mail Transfer Protocol". It is utilised to handle the sending of emails. In order to support email services, a protocol pair is required, comprising of SMTP and POP/IMAP. Together they allow the user to send outgoing mail and retrieve incoming mail, respectively.

The SMTP server performs three basic functions:

     It verifies who is sending emails through the SMTP server.
     It sends the outgoing mail
     If the outgoing mail can't be delivered it sends the message back to the sender



We can map the journey of an email from your computer to the recipient’s like this:


1. The mail user agent, which is either your email client or an external program. connects to the SMTP server of your domain, e.g. smtp.google.com. This initiates the SMTP handshake. This connection works over the SMTP port- which is usually 25. Once these connections have been made and validated, the SMTP session starts.


![Pasted image 20241003090610](https://github.com/user-attachments/assets/8309fba5-8b70-4747-9589-719cd4a3c934)


3. The process of sending mail can now begin. The client first submits the sender, and recipient's email address- the body of the email and any attachments, to the server.

4. The SMTP server then checks whether the domain name of the recipient and the sender is the same.

5. The SMTP server of the sender will make a connection to the recipient's SMTP server before relaying the email. If the recipient's server can't be accessed, or is not available- the Email gets put into an SMTP queue.

6. Then, the recipient's SMTP server will verify the incoming email. It does this by checking if the domain and user name have been recognised. The server will then forward the email to the POP or IMAP server, as shown in the diagram above.

7. The E-Mail will then show up in the recipient's inbox.


### Enumerating SMTP:



Nmap:

nmap -sV -sC 10.10.246.91 -nnvvv


![Pasted image 20241003092220](https://github.com/user-attachments/assets/3fc3e53b-268e-4b5b-91ea-76d3cccf73d0)


nmap --script smtp* -p 25 10.10.246.91 -nnvvv 


![Pasted image 20241003093718](https://github.com/user-attachments/assets/eaf98569-4000-4f77-9362-9e3b01f4dc3d)




![Pasted image 20241003093831](https://github.com/user-attachments/assets/83a0a6b4-ef49-418e-b832-fd192341d2f1)


PORT   STATE SERVICE REASON         VERSION
25/tcp open  smtp  Postfix smtpd
 commonName=polosmtp




### Enumerating with Telnet:


The simplest techniques to query the SMTP server is telnet with VRFY, EXPN and RCPT TO commands, those commands will answer if the username entered exist or not, 252 code if the user exists, 550 code if the user is unknown.



![Pasted image 20241003093336](https://github.com/user-attachments/assets/0694813a-c3a7-48c1-8f9a-285d1179e49b)


# Network traffic:


![Pasted image 20241003093435](https://github.com/user-attachments/assets/d5ad241d-29c9-41a7-9ee5-3237c0a63d37)



![Pasted image 20241003094343](https://github.com/user-attachments/assets/2bdac4b0-dcc5-4fcc-a1d4-86ff6b0b8e71)


# Detecting smtp enumeration:

Filters:

 smtp.req.parameter
 smtp.req.parameter
 

![Pasted image 20241003103244](https://github.com/user-attachments/assets/dfd9b2e5-2f94-4763-962b-7fe4cd7e5e10)

SMTP codes link to world, codes message "root" are anomalies or indicator of foot printing
 

![Pasted image 20241003094518](https://github.com/user-attachments/assets/c991c19c-1c62-40b8-ac68-af28f44e239c)





# METASPLOIT:


![Pasted image 20241003091852](https://github.com/user-attachments/assets/24aa3ab5-80eb-42a7-8570-8475cd523df6)


Using scanner SMTP Auxiliary Modules:



![Pasted image 20241003095321](https://github.com/user-attachments/assets/bb82bc66-210f-41bb-aa9d-ec18a8fd6bd7)



# smtp_version:

Poorly configured or vulnerable mail servers can often provide an initial foothold into a network but prior to launching an attack, we want to fingerprint the server to make our targeting as precise as possible. The smtp_version module, as its name implies, will scan a range of IP addresses and determine the version of any mail servers it encounters.

search smtp_version


![Pasted image 20241003100626](https://github.com/user-attachments/assets/8166119d-54d9-47dd-952d-adb9a193d756)

use auxiliary/scanner/smtp/smtp_version

show options
 set RHOSTS 10.10.246.91
  set THREADS 254
   run
# smtp_enum

The SMTP Enumeration module will connect to a given mail server and use a wordlist to enumerate users that are present on the remote system.

search smtp_enum


![Pasted image 20241003095709](https://github.com/user-attachments/assets/6e5698fd-9ad1-4871-a4b8-98ba3383909c)


use auxiliary/scanner/smtp/smtp_enum
show options
set RHOSTS 10.10.246.91
 run


![Pasted image 20241003101932](https://github.com/user-attachments/assets/4f6fea0c-4014-4042-aafc-00460dea8627)


### Password list:
```
 _apt, administrator, backup, bin, daemon, dnsmasq, games, 
 gnats, irc, landscape, list, lp, lxd, mail, man, messagebus, 
 news, nobody, pollinate, postfix, postmaster, proxy, sshd, sync, 
 sys, syslog, systemd-network, systemd-resolve, systemd-timesync, 
 uucp, uuidd, www-data
```


### Using a world list:

set USER_FILE /usr/share/wordlists/SecLists/Usernames 

 At the end of our Enumeration section we have a few vital pieces of information:

A user account name :  

administrator

The type of SMTP server and Operating System running:  

polosmtp.home; OS: Linux; CPE: cpe:/o:linux:linux_kernel


### Exploiting SMTP:

# Hydra:



![Pasted image 20241003103939](https://github.com/user-attachments/assets/bcf3a888-263e-4458-b78e-cc5c3314c47f)


User list:


![Pasted image 20241003105302](https://github.com/user-attachments/assets/ef1880ca-05f0-406b-bf8a-82727dba0465)



# SSH  password attack:


31 users on my list against 14344391 passwords


```
hydra -t 16 -L users.txt -P /usr/share/wordlists/rockyou.txt -vV 10.10.246.91  ssh
```



![Pasted image 20241003105819](https://github.com/user-attachments/assets/6a0a0c37-9aa5-41bb-bb88-4a4bf5c4c202)



![Pasted image 20241003111546](https://github.com/user-attachments/assets/36c2cb35-8279-4e3b-95d8-870edb615094)


### Login into the smtp server:

ssh admistrator@10.10.246.91


![Pasted image 20241003113140](https://github.com/user-attachments/assets/72e2f246-e283-45e0-a570-68963c4f2c30)

# Detecting SSH password attacks, protocol anomalies :



### ERROR_SSH_KEY_EXCHANGE_FAILED
code: 103 (0x0067)
Key exchange failed



![Pasted image 20241003110641](https://github.com/user-attachments/assets/70e55576-740b-4241-9319-5efcc04bcc3f)


## ERROR_SSH_AUTHENTICATION_FAILED
code :10 (0x000A)
Authentication failed. There could be wrong password or something else


![Pasted image 20241003110612](https://github.com/user-attachments/assets/073c816b-9906-49b1-97af-2c0ae2320cfa)



## ERROR_SSH_HOST_NOT_ALLOWED_TO_CONNECT
code :101 (0x0065)
Connection was rejected by remote host

Many connection with short duration can indicate  anomalies on the SSH server:



![Pasted image 20241003110347](https://github.com/user-attachments/assets/bc73fc40-c769-4121-9211-2c7af6347ee3)
