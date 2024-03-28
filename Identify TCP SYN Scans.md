# TCP Attacks and Anomoly Behavior

TCP (Transmission Control Protocol) is a fundamental protocol used for reliable data transmission between devices. However, TCP is also a common target for various attacks aimed at disrupting communication, exploiting vulnerabilities, or gaining unauthorized access. When analyzing network anomalies, starting with the IP layer is crucial as it facilitates packet transfer using source and destination IP addresses. While the IP layer doesn't manage packet loss or tampering, higher layers like transport and application layers handle these issues.

Here we take a look at the most common TCP attacks, highlighting the importance of effective network security practices. Implementing firewalls, intrusion detection systems (IDS), access control lists (ACLs), and regular security audits are crucial in mitigating the risks associated with TCP attacks and maintaining a secure network environment.
## Scenario 1: TCP SYN Scan

Let's take a look at the folowing pcap and the abnormal TCP behavior. To start, we will Filter for tcp syn flags and ports (1-1024):

```
tcpdump -nr threat_actor.pcap tcp[tcpflags] == tcp-syn and portrange 1-1024
```

![Pasted image 20240325120655](https://github.com/lm3nitro/Projects/assets/55665256/5faa423d-3c40-4433-b7e0-9b5b151f057c)

It appears that the threat actor located at 192.168.11.62 is actively scanning the node 192.168.11.46 in search of open ports. We have observed a significant number of SYN (synchronize) requests being sent to 192.168.11.46 across multiple ports within a fraction of a second.

To pinpoint the extent of the synchronization requests sent by the threat actor, we need to narrow down our analysis to determine the exact number of SYN requests received by 192.168.11.46.

```
tcpdump -nr threat_actor.pcap src host 192.168.11.62 and dst host 192.168.11.62 and dst host 192.168.11.46 and tcp[tcpflags] == tcp-syn | wc -l
```

![Pasted image 20240325122547](https://github.com/lm3nitro/Projects/assets/55665256/0454d367-56d4-49c1-bd12-5f51240e92fa)

## Scenario 2: TCP ACK Scan

In the following pcap, we see the host 192.168.10.5 sending a large amount of ACK packets to our host 192.168.10.1. These packets have some unusual traffic as they are all originating from the same source port and going to the same IP address but in different ports. We can also see the the sequence number remains the same throughout all of the packets. 

```
tcpdump -nr threat_actor.pcap tcp[tcpflags] == tcp-ack and portrange 1-65365
```

![Pasted image 20240327153755](https://github.com/lm3nitro/Projects/assets/55665256/443bb5d8-effc-44b1-9ced-dbbe2fc9ab57)

When analyzing a TCP ACK scan using tcpdump, look for packets with the ACK flag set, targeting specific port ranges commonly used by the scanning device, and observe timing patterns, and response behaviors.

## Scenario 3: TCP FIN Scan

A TCP FIN scan is a port scanning technique where the attacker sends TCP packets with the FIN (finish) flag set to closed ports on a target system. The goal of a FIN scan is to determine whether a port is open, closed, or filtered by examining the responses received from the target.

Threat actors might use a TCP FIN scan for several reasons:

+ Stealthy Scanning
+ Port State Identification
+ Firewall Evasion
+ Operating System Fingerprinting
+ Anomaly Detection Testing

Lets look at an example of this type of scan in the pcap below:

```
tcpdump -nr threat_actor.pcap tcp[tcpflags] == tcp-fin -c 20
```

![Pasted image 20240327154309](https://github.com/lm3nitro/Projects/assets/55665256/1895a7b0-0a00-432a-badb-3eae4205eecf)

Overall, TCP FIN scans are a reconnaissance tool used by threat actors to gather information about a target system's port states, network configuration, and potential vulnerabilities while attempting to remain undetected and evade security measures.

## Scenario 4: TCP Xmas Scan

A TCP Xmas scan is a port scanning technique where the attacker sends TCP packets with specific flags set to closed ports on a target system. The flags typically include the FIN (finish), PSH (push), and URG (urgent) flags, creating a packet with all flags "lit up" like a Christmas tree. 

In the example below, we can take a look and see the behavior described above.

```
tcpdump -nr threat_actor.pcap 'tcp[13] & 32!=0' or tcp[13] & 1!=0' -c 20
```

![Pasted image 20240327155225](https://github.com/lm3nitro/Projects/assets/55665256/a6434005-6b76-438c-b3a1-a6c7435cf034)


## Scenario 5: TCP Null Scan

Null scans with flags set to none are often used by attackers for reconnaissance purposes to gather information about a target system's port states and network configuration. They are considered stealthy because they do not initiate a full TCP connection or include any specific flag information that may trigger detection by intrusion detection systems (IDS) or firewall rules.

Lets take a look to see what this looks like in a pcap and how we can identify it. 

```
tcpdump -nr threat_actor.pcap ! 'tcp[13] & 32!=0' or 'tcp[13] & 1!=0' -c 10
```

![Pasted image 20240327155836](https://github.com/lm3nitro/Projects/assets/55665256/87b22a4a-5311-48c2-b4ce-18f3005eba0c)

Unlike other port scanning techniques that may set specific flags to elicit responses from the target system, a null scan with flags set to none deliberately omits any flag information, creating a "null" or empty TCP packet.

## Scenario 6: TCP Fragmentation Scan

A TCP fragmentation scan is a port scanning technique that exploits vulnerabilities in the TCP/IP packet fragmentation and reassembly process. In this type of scan, the attacker sends specially crafted fragmented packets to a target system, aiming to bypass firewall rules, evade intrusion detection systems (IDS), or identify open ports.

Below is an example of this type of scan.

```
tcpdump -nr threat_actor.pcap and 'ip[6] = 32' -c 20
```

![Pasted image 20240327160923](https://github.com/lm3nitro/Projects/assets/55665256/289bd943-eaba-4d52-8982-b7a1c1e31ffa)

Now lets take a look at the following fields: Flags, TTL, and Length

```
tcpdump -nr threat_actor.pcap and 'ip[6] = 32' -c 3 -vvvA
```

![Pasted image 20240327161124](https://github.com/lm3nitro/Projects/assets/55665256/82fa873b-031b-4f54-bfca-64be7e1a99fa)

In a TCP fragmentation scan, it's crucial to consider the Flags field in the TCP header for identifying packet purpose and nature. Analyzing the TTL (Time to Live) field in the IP header provides insights into network path anomalies, while monitoring packet Length helps detect manipulation attempts and potential vulnerabilities in packet reassembly. These three aspects—Flags, TTL, and Length—are key factors to consider for effective analysis and mitigation of TCP fragmentation scan attacks.

## Scenario 7: TCP Reset Scans

If an adversary aims to disrupt our network with a denial-of-service attack, they might use a TCP RST Packet injection attack, also known as TCP connection termination. This attack involves spoofing the source address to the affected machine's IP, modifying TCP packets with the RST flag to terminate connections, and targeting ports currently in use by our machines.

```
tcpdump -nr 'tcp[13] & 4!=0' && ! net 10.0.0.0/8
```
![Pasted image 20240327161657](https://github.com/lm3nitro/Projects/assets/55665256/404ceed5-b5a4-4de2-8c07-833817cddf31)

This attack involves several components:

+ The attacker spoofs the source address to match the affected machine's.
+ The attacker alters the TCP packet to include the RST flag, ending the connection.
+ The attacker designates the destination port to match one currently utilized by one of our machines.

One way to identify a TCP RST attack is by comparing the physical address of the transmitter with our network device list; a mismatch, such as a different MAC address, indicates potential malicious activity. However, attackers can spoof MAC addresses to evade detection, leading to retransmissions and other anomalies similar to those seen in ARP poisoning incidents.

## Scenario 8: LAN-DoS Attack

![Pasted image 20240327163548](https://github.com/lm3nitro/Projects/assets/55665256/7bbb6ac9-69b5-4084-b351-0b4c04b54a0d)



Identify TCP SYN SCAN with a  Random spoof source:

![Pasted image 20240327164544](https://github.com/lm3nitro/Projects/assets/55665256/30414810-fef8-43bc-8f34-6f853194ad9d)




The attacker is scanning an internal  web sever on port 80 by spoofing the source with ramdom IPs  to hide the source of the attack, Also it seens like the attacker inside the network since all the ramdom IPs have a TTL of 64

![Pasted image 20240327165516](https://github.com/lm3nitro/Projects/assets/55665256/32ba57a4-79de-4b4c-8b3d-09a13ee98f7f)


## Scenario 9: TCP Hijacking:

![Pasted image 20240327175000](https://github.com/lm3nitro/Projects/assets/55665256/e783d2df-687b-4e13-bab9-23239141d63e)


The attacker will need to block ACKs from reaching the affected machine in order to continue the hijacking. They do this either through delaying or blocking the ACK packets. As such, this attack is very commonly employed with ARP poisoning, and we might notice the following in our traffic analysis.

Identify a large number of ARP requests coming from one node by monitoring ARP protocol.

![Pasted image 20240327200915](https://github.com/lm3nitro/Projects/assets/55665256/de3f64f9-2f34-4363-8d51-ca27913bd238)


In this screenshot show all the ARP request send by  08:00:27:53:0c:ba to  ff:ff:ff:ff:ff:ff :

![Pasted image 20240327202326](https://github.com/lm3nitro/Projects/assets/55665256/b380cb11-5a73-4c7e-b21a-a2ab71516b6b)


The node is flooding the network with ARP request asking where every IP node is at :


![Pasted image 20240327202448](https://github.com/lm3nitro/Projects/assets/55665256/657d58ba-7fc6-4b7d-85eb-3dcd3d08c4c3)


 In this case we see that 192.168.10.5 is sending a large amount of request to all the nodes on the network with the MAC ending in 0c:ba , later the same node impersonate the node with the IP 192.168.10.1 with the MAC ending in b9:4f. At this point the attacker can impersonate any node on the network or create MIT attack to make every request to go trough the attacker.


![Pasted image 20240327181139](https://github.com/lm3nitro/Projects/assets/55665256/59bdf099-e010-4eb0-a54c-a5ec4bbff16d)


By actively monitor the target connection they want to hijack the attacker will then conduct sequence number prediction in order to inject their malicious packets in the correct order. During this injection they will spoof the source address to be the same as our affected machine.  

![Pasted image 20240327174332](https://github.com/lm3nitro/Projects/assets/55665256/43ccec86-983b-4465-aa93-2b2263860f25)


To detect TCP session hijacking using TCP header flags, it is necessary to monitor and analyse the TCP traffic between the hosts for any anomalies or discrepancies in the TCP header flags. An example of this would be unexpected or repeated SYN, ACK, FIN, or RST flags, which can indicate an attempt to initiate, acknowledge, end, or reset a TCP connection without following the normal protocol. Additionally, mismatched or out-of-order sequence numbers may suggest an attempt to inject or modify the TCP packets. Inconsistent or missing PSH or URG flags may also point to an attempt to alter the data or priority of the TCP packets. By monitoring for these signs of TCP session hijacking using TCP header flags, it is possible to detect malicious activity and protect against potential attacks.

Protect against TCP/IP hijacking:

To protect yourself against TCP/IP hijacking, you can take several measures. First, use a reliable antivirus software that can detect and prevent packet sniffing and injection. Second, use encryption technologies such as VPNs or SSL/TLS to encrypt your network traffic. Finally, practice good security hygiene, such as regularly updating your software and using strong passwords.


