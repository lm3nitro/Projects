
### SMB

SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network.

Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers.

The SMB protocol is known as a response-request protocol, meaning that it transmits multiple messages between the client and server to establish a connection. Clients connect to servers using TCP/IP (actually NetBIOS over TCP/IP as specified in RFC1001 and RFC1002), NetBEUI or IPX/SPX. 


![Pasted image 20240918122330](https://github.com/user-attachments/assets/4e0401ee-d22c-49ca-ac2e-52c4f06f4b89)

Once they have established a connection, clients can then send commands (SMBs) to the server that allow them to access shares, open files, read and write files, and generally do all the sort of things that you want to do with a file system. However, in the case of SMB, these things are done over the network. 

### Enumeration:

Enumeration is the process of gathering information on a target in order to find potential attack vectors and aid in exploitation.
### Using Enum4Linux to enumerate SMB:

Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB.

### Version: 
enum4linux v0.8.9


![Pasted image 20240918122848](https://github.com/user-attachments/assets/349b4c5b-f424-404b-be1b-1b6a11ee7b54)



### Conducting an nmap scan to see how many the ports are open:

nmap -sV 10.10.107.71 -vvv


![Pasted image 20240918123301](https://github.com/user-attachments/assets/5d83727c-9990-4484-b23c-ec0daa9bd1e3)


### Using Nmap NSE scripts to detect misconfigurations on the SMB service:

nmap -sU -sS --script smb-enum-shares.nse -p 445,139

![Pasted image 20240918141635](https://github.com/user-attachments/assets/4ae32adf-7f00-4918-a81d-1b7192317bf5)

### Detecting the nmap scan:

SSH


![Pasted image 20240918124957](https://github.com/user-attachments/assets/1c598dc8-d57e-4c49-906a-cc27b18e0fb7)


SMB 

![Pasted image 20240918124712](https://github.com/user-attachments/assets/b8bf4ef8-15ba-4580-846b-523712e1e985)


![Pasted image 20240918141815](https://github.com/user-attachments/assets/262b3c5b-82be-4950-99f7-b87ec472fc50)


![Pasted image 20240918124838](https://github.com/user-attachments/assets/4ce78237-67b5-44b3-b63d-91cbffdaa3f0)



![Pasted image 20240918124816](https://github.com/user-attachments/assets/8ace7923-82d3-4e29-bdc9-8b275bf2b927)

I was able to collect the name of the host , the SMB version, OS information and the name of the group.

Host: POLOSMB
System: Linux
Service: Samba SMBd 3.x -4.x
Workgroup name: workgroup


enum4linux -a  10.10.107.71


![Pasted image 20240918125326](https://github.com/user-attachments/assets/0ceb4001-aeae-44b9-b429-268f0175ec44)



![Pasted image 20240918125345](https://github.com/user-attachments/assets/c6fbc759-1622-4a5c-811f-c43f4d0938db)



![Pasted image 20240918125405](https://github.com/user-attachments/assets/a10f7bec-485b-42f8-bf0e-a2acbb87cf05)



![Pasted image 20240918125424](https://github.com/user-attachments/assets/d01175f4-b693-4006-bd04-379b03893568)



![Pasted image 20240918125502](https://github.com/user-attachments/assets/74a21ab1-5bb3-4596-b668-c4c99452a234)



### Information about the scan:

Operating system version: 6.1
Linux version: Ubuntu
Shares: profiles


### Detecting SMB enumeration:

The client is sending many request at once, this is consider abnormal SMB traffic behaviour:

The SMB2 CREATE Request packet is sent by a client to request either creation of or access to a file. In case of a named pipe or printer, the server MUST create a new file. 

![Pasted image 20240918130154](https://github.com/user-attachments/assets/c9040af2-158f-4fcf-ac4a-b9700637894c)


![Pasted image 20240918131227](https://github.com/user-attachments/assets/d741377b-d810-481e-932f-5bf59595c535)


Another indicator is The LsaOpenPolicy function wich opens a handle to the Policy object on a local or remote system.

You must run the process "As Administrator" so that the call doesn't fail with ERROR_ACCESS_DENIED.


![Pasted image 20240918130632](https://github.com/user-attachments/assets/c2e2b9ed-27d9-4e97-be01-6bf6c87ea09d)


### Unique conceptions:



![Pasted image 20240918125842](https://github.com/user-attachments/assets/6a7aa08b-21e4-4647-945b-52bb7b4453e7)

### SMB traffic graph:




![Pasted image 20240918125918](https://github.com/user-attachments/assets/11bdbf44-07de-41e4-a218-605a96a83871)



### Exploiting SMB

 The best way into a system is due to misconfigurations in the system. In this case, we're going to be exploiting anonymous SMB share access- a common misconfiguration that can allow us to gain information that will lead to a shell.


### Using SMBclient to access the SMB shares:



![Pasted image 20240918131830](https://github.com/user-attachments/assets/7a5a7496-a5a4-4ee4-a09f-e641ced903e6)


-U [name] : to specify the user

-p [port] : to specify the port

 smbclient //[IP]/[SHARE] 


 Verifying if the share has been configured to allow anonymous access, if it doesn't require authentication to view the files. We can do this easily by:

- using the username "Anonymous"

- connecting to the share we found during the enumeration stage

- and not supplying a password. 

smbclient //10.10.107.71/profiles


smbclient //ip_or_fqdn/share_name -U username -p port_number


![Pasted image 20240918132230](https://github.com/user-attachments/assets/2f7447e7-fb02-44b8-be47-e2876a3eb098)




![Pasted image 20240918132207](https://github.com/user-attachments/assets/6d1a6709-89a7-4316-85de-ea58ef58f287)



Using command dir to list the files on the directory


![Pasted image 20240918133240](https://github.com/user-attachments/assets/84ddb6d9-d6e7-4e40-8dda-01552ce61ffd)


Using command more to open files on the SMB

more "Working From Home Information.txt"


![Pasted image 20240918133335](https://github.com/user-attachments/assets/3c38dea6-ffab-4556-b283-7ba802035730)


Using "cd" command to access directories




![Pasted image 20240918133857](https://github.com/user-attachments/assets/8d924766-444e-45a5-a399-ae675497b654)


Using "get" command to down files


![Pasted image 20240918133959](https://github.com/user-attachments/assets/2b09d34f-04b8-4b89-88bb-53b8a11f61a5)


![Pasted image 20240918134641](https://github.com/user-attachments/assets/902d71f4-af36-49ba-b5bc-70c5753623d6)

Verify the public key with command "cat"

![Pasted image 20240918134820](https://github.com/user-attachments/assets/f5a0052f-a4c4-4ce5-bca3-0ae955bbbdf2)


The public key belongs to a user call "cactus"

### Connecting to the server using SSH:

I was able to get some information about the user called John, cactus and some rsa private and pub keys. 

changing the permission of the public key with "chmod "command: 

chmod 600 id_rsa.pub

ssh -i id_rsa.pub cactus@10.10.107.71


![Pasted image 20240918135323](https://github.com/user-attachments/assets/009a2270-c983-4735-8a25-2d2027ab974c)

