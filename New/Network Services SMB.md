# SMB

SMB, or Server Message Block, is a network protocol primarily used for sharing files, printers, and other resources between computers on a network. It facilitates communication between clients and servers and is commonly used in Windows environments, though it is also supported by other operating systems. Once connected to the shared resource, the client can send additional requests to read, write, or manage files within that resource. The server processes these requests and responds accordingly.

![Pasted image 20240918122330](https://github.com/user-attachments/assets/4e0401ee-d22c-49ca-ac2e-52c4f06f4b89)

### Scope:

In this exercise, I will be performing enumeration on a server running the SMB service to identify any misconfigurations. Once the misconfigurations are identified, I will be exploiting them and gaining access to the target host.  

### Tools and Technology
Linux, Wireshark, and SMB

## Enumeration:

To enumerate SMB, I will be using Enum4Linux and Nmap. Enum4linux is a powerful tool used for gathering information about SMB on both Windows and Linux systems. I first started by using Nmap to scan the target host and see which ports are open:

```
nmap -sV 10.10.107.71 -vvv
```

![Pasted image 20240918123301](https://github.com/user-attachments/assets/5d83727c-9990-4484-b23c-ec0daa9bd1e3)

Based on the output, ports 22 (SSH) and 139/445 (SMB) are showing as open. I then used the Nmap NSE script to detect misconfigurations on the SMB service:

```
nmap -sU -sS --script smb-enum-shares.nse -p 445,139
```

![Pasted image 20240918141635](https://github.com/user-attachments/assets/4ae32adf-7f00-4918-a81d-1b7192317bf5)

The output above shows the misconfiguration. It shows that anonymous has access to read/write. 

## Wireshark:

While performing these actions, I also had Wireshark running in order to analyze the traffic and see how these scans work and see how to identify this type of traffic:

Scanning for SSH:

![Pasted image 20240918124957](https://github.com/user-attachments/assets/1c598dc8-d57e-4c49-906a-cc27b18e0fb7)

Scanning for SMB:

![Pasted image 20240918124712](https://github.com/user-attachments/assets/b8bf4ef8-15ba-4580-846b-523712e1e985)

SMB traffic:

![Pasted image 20240918141815](https://github.com/user-attachments/assets/262b3c5b-82be-4950-99f7-b87ec472fc50)

![Pasted image 20240918124838](https://github.com/user-attachments/assets/4ce78237-67b5-44b3-b63d-91cbffdaa3f0)

Closer look at one of the packets:

![Pasted image 20240918124816](https://github.com/user-attachments/assets/8ace7923-82d3-4e29-bdc9-8b275bf2b927)

Based on the traffic seen above, I was able to collect the folllowing:

+ Host: POLOSMB
+ Operating Sytem: Linux
+ Service: Samba SMBd 3.x -4.x
+ Workgroup Name: workgroup

Next, I used enum4linux. The version that I will be using is v0.8.9:

![Pasted image 20240918122848](https://github.com/user-attachments/assets/349b4c5b-f424-404b-be1b-1b6a11ee7b54)

```
enum4linux -a  10.10.107.71
```

![Pasted image 20240918125326](https://github.com/user-attachments/assets/0ceb4001-aeae-44b9-b429-268f0175ec44)

More information on the target:

![Pasted image 20240918125345](https://github.com/user-attachments/assets/c6fbc759-1622-4a5c-811f-c43f4d0938db)

Operating system information:

![Pasted image 20240918125405](https://github.com/user-attachments/assets/a10f7bec-485b-42f8-bf0e-a2acbb87cf05)

Available sharenames:

![Pasted image 20240918125424](https://github.com/user-attachments/assets/d01175f4-b693-4006-bd04-379b03893568)

Groups:

![Pasted image 20240918125502](https://github.com/user-attachments/assets/74a21ab1-5bb3-4596-b668-c4c99452a234)

From the scan I was able to gather several things:

+ Operating system version: 6.1
+ Linux OS: Ubuntu
+ Shares: It has a `profiles` sharename that has user profiles

## Detecting SMB Enumeration:

Taking another look at Wireshark, this is what I was able to see. Based on the output, the client is sending many requests at once, this is considered abnormal SMB traffic behavior:

![Pasted image 20240918130154](https://github.com/user-attachments/assets/c9040af2-158f-4fcf-ac4a-b9700637894c)

I also see many `Create Request File` related traffic. This type of packet is sent by a client to request either the creation of or access to a file. 

![Pasted image 20240918131227](https://github.com/user-attachments/assets/d741377b-d810-481e-932f-5bf59595c535)

Another indicator is The `LsaOpenPolicy` function which opens a handle to the policy object on a local or remote system.

![Pasted image 20240918130632](https://github.com/user-attachments/assets/c2e2b9ed-27d9-4e97-be01-6bf6c87ea09d)

I also took a look at the number of unique connections:

![Pasted image 20240918125842](https://github.com/user-attachments/assets/6a7aa08b-21e4-4647-945b-52bb7b4453e7)

SMB traffic graph:

![Pasted image 20240918125918](https://github.com/user-attachments/assets/11bdbf44-07de-41e4-a218-605a96a83871)

## Exploiting SMB

Exploits usually happen and can be caused due to misconfigurations in the system, application, or services. In this case, I will be exploiting the anonymous SMB share access- a common misconfiguration that can allow us to gain access.

I will be using SMBclient to access the SMB shares:

![Pasted image 20240918131830](https://github.com/user-attachments/assets/7a5a7496-a5a4-4ee4-a09f-e641ced903e6)

There are several ways to connect, one where you specify the user and another where you just specify the IP and share. 

Option 1: smbclient //[IP]/[SHARE]

Option 2: smbclient //ip_or_fqdn/share_name -U username -p port_number
+ -U: specify the user
+ -p: specify the port

In this case, I just used option 1:

```
smbclient //10.10.107.71/profiles
```

I was prompted for a password, but hit enter as there wa no password configured:

![Pasted image 20240918132230](https://github.com/user-attachments/assets/2f7447e7-fb02-44b8-be47-e2876a3eb098)

Looking at Wireshark, I can see that the connection was established:

![Pasted image 20240918132207](https://github.com/user-attachments/assets/6d1a6709-89a7-4316-85de-ea58ef58f287)

Using command `DIR` to list the files on the directory:

![Pasted image 20240918133240](https://github.com/user-attachments/assets/84ddb6d9-d6e7-4e40-8dda-01552ce61ffd)

I see that there is a txt that looks like it might contain some useful information. I used the command `more` to the file on the SMB:

```
more "Working From Home Information.txt"
```

![Pasted image 20240918133335](https://github.com/user-attachments/assets/3c38dea6-ffab-4556-b283-7ba802035730)

This provided me with 2 possible users, John and James. I then went back to the list of directories and navigated to the .ssh directory:

![Pasted image 20240918133857](https://github.com/user-attachments/assets/8d924766-444e-45a5-a399-ae675497b654)

I then used the `get` command to download files:

![Pasted image 20240918133959](https://github.com/user-attachments/assets/2b09d34f-04b8-4b89-88bb-53b8a11f61a5)

![Pasted image 20240918134641](https://github.com/user-attachments/assets/902d71f4-af36-49ba-b5bc-70c5753623d6)

Next, I verified the public key with command `cat`:

![Pasted image 20240918134820](https://github.com/user-attachments/assets/f5a0052f-a4c4-4ce5-bca3-0ae955bbbdf2)

The public key belongs to a user called "cactus" which references "John Cactus" from the output above.

## SSH:

After getting information about the user "John Cactus", I tried to SSH into the server as the user. First, I changed the permissions of the public key: 

```
chmod 600 id_rsa.pub
```

Next SSH:

```
ssh -i id_rsa.pub cactus@10.10.107.71
```

![Pasted image 20240918135323](https://github.com/user-attachments/assets/009a2270-c983-4735-8a25-2d2027ab974c)

I was able to successfully authenticate as the user1

### Summary:

In this exercise, I was able to scan the target host and perform enumeration of the SMB service that it was running. This then allowed me to connect to the SMB share and download SSH keys that were then used to authenticate as the user "John Cactus" and successfully SSH into the server. In doing this, I was able to learn how to exploit a SMB misconfiguration which allowed any user to access the share. I was also able to view and analyze the traffic in Wireshark which allowed me to see how this type of attack happens and how to identify it. 

As mitigation against this type of attack, it's important to limit permissions using the principle of least privilege, implement strong authentication methods, and configure firewalls to restrict SMB traffic to trusted networks. Additionally, I suggest enabling logging for monitoring access. 

