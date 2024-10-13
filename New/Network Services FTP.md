


![Pasted image 20240919133005](https://github.com/user-attachments/assets/a0751fbc-f431-40a3-aab8-7570c72c9f0f)

File Transfer Protocol (FTP) is, as the name suggests , a protocol used to allow remote transfer of files over a network. It uses a client-server model to do this, and- as we'll come on to later- relays commands and data in a very efficient way.

The FTP server may support either Active or Passive connections, or both.  

In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it. 

In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it.  


### Enumerating FTP:


nmap -Pn -sS 10.10.73.116 -p- -T5 -vvvv 


![Pasted image 20240919134047](https://github.com/user-attachments/assets/8c887aa2-2452-4beb-ac6f-6a2ef84e9a0e)



![Pasted image 20240919134313](https://github.com/user-attachments/assets/62c0580c-e92a-4386-af16-1ef9bfdb154e)

nmap -A -sC 10.10.73.116 -p21 -vvvv 


![Pasted image 20240919134513](https://github.com/user-attachments/assets/5d5dc7c8-4375-452b-811b-07d8d7a6e53b)


### Information about the target:

Service: FTP
Port: 21
Version: vsftpd 2.0.8
OS: Linux

Note: It seems like ftp-anon: Anonymous FTP login allowed (FTP code 230)
### Detecting FTP enumeration:

TCP traffic:


![Pasted image 20240919135553](https://github.com/user-attachments/assets/29ca822e-99b7-47a6-9257-5369bf7e7157)


 ICMP traffic:
 

![Pasted image 20240919135953](https://github.com/user-attachments/assets/74317e5e-faee-4cbc-a530-f7fcb15633b6)


FTP requests:



![Pasted image 20240919134950](https://github.com/user-attachments/assets/302295c9-4e87-4432-a6cb-528f834f8199)


FTP responses:

Codes:


![Pasted image 20240919145308](https://github.com/user-attachments/assets/6d06febd-dff7-41f6-a55d-16265d563220)


![Pasted image 20240919135745](https://github.com/user-attachments/assets/8564b1e5-dd23-4633-abdd-da473d5c6fd9)


Payload decoding:


![Pasted image 20240919135658](https://github.com/user-attachments/assets/d3dc2e13-7626-4077-a3a5-0c3df12c530f)


### Login into the FTP server:

ftp 10.10.73.116


![Pasted image 20240919142356](https://github.com/user-attachments/assets/e855ceba-0192-4761-9973-2ec2bca56bf6)


### Listing file and downloading :


![Pasted image 20240919142834](https://github.com/user-attachments/assets/cec12814-61b1-4ae6-a2fb-07af24704a03)


![Pasted image 20240919142909](https://github.com/user-attachments/assets/814cf631-344e-43cf-b848-4162ab89037a)


Looking the connected of the file download from the ftp server:


![Pasted image 20240919151001](https://github.com/user-attachments/assets/c0ba9c2a-3aff-44f5-9759-e611ec0115e3)


### Decode network traffic:


![Pasted image 20240919143018](https://github.com/user-attachments/assets/3e308adc-df69-4881-a509-f6bfa7527b99)


### Using Hydra for dictionary attack:


![Pasted image 20240919150644](https://github.com/user-attachments/assets/371e9ac5-d187-4ff3-9d79-0d114e150b76)


Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.

hydra -t 4 -l mike -P /usr/share/wordlists/rockyou.txt -vV 10.10.73.116 ftp


![Pasted image 20240919151353](https://github.com/user-attachments/assets/a112df38-e363-4b65-9d35-a45b4757b55b)


#### Identifying dictionary/brute force attacks: 


![Pasted image 20240919151823](https://github.com/user-attachments/assets/6f8bc2df-2b1c-482f-9b55-8e1718c28946)


