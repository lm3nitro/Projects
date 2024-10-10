# TCP Anomolies

TCP (Transmission Control Protocol) is a fundamental protocol used for reliable data transmission between devices. However, TCP is also a common target for various attacks aimed at disrupting communication, exploiting vulnerabilities, or gaining unauthorized access. When analyzing network anomalies, starting with the IP layer is crucial as it facilitates packet transfer using source and destination IP addresses. While the IP layer doesn't manage packet loss or tampering, higher layers like transport and application layers handle these issues.

### Scope:

Here we take a look at the most common TCP attacks and anomolies, highlighting the importance of effective network security practices. Implementing firewalls, intrusion detection systems (IDS), access control lists (ACLs), and regular security audits are crucial in mitigating the risks associated with TCP attacks and maintaining a secure network environment.

Below, I will demonstrate the following:

+ TCP SYN Scan
+ TCP ACK Scan
+ TCP FIN Scan
+ TCP Xmas Scan
+ TCP Null Scan
+ TCP Fragmentation Scan
+ TCP Reset Scans
+ LAN-DoS Attack
+ TCP SYN flood with Port Sweep
+ TCP Hijacking




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

In a LAND attack, the attacker sends specially crafted packets with the same source and destination IP addresses as well as the same source and destination port numbers. This can confuse the target device's TCP/IP stack, causing it to enter a loop where it keeps processing the malicious packets, consuming CPU resources and potentially leading to a denial of service.

Lets see an example below:
```
tcpdump -nr threat_actor.pcap 'tcp[13] $ 2!=0' and net 192.168.10.0/24 -c 20
```

![Pasted image 20240327163548](https://github.com/lm3nitro/Projects/assets/55665256/7bbb6ac9-69b5-4084-b351-0b4c04b54a0d)


Here we see traffic originating from the same ip address as the destination with incremental source ports. This makes it seem like the attack is coming from the target itself. Overall, timely identification of LAND attacks and other DoS attacks is crucial for maintaining network security, service availability, and business continuity.

## Scenario 9: TCP SYN flood with Port Sweep

If the source ports in a TCP SYN flood attack with random spoofed addresses are incremented in value, it adds another layer of complexity to the attack and is often referred to as a "port sweep" or "port scan" within the context of the SYN flood. In this scenario, the attacker not only floods the target with a large number of TCP SYN packets but also varies the source ports in an incremental manner.
```
tcpdump -nr threat_actor.pcap 'tcp[13] $ 2!=0' and ! src net 192.168.10.0/24 -c 20
```

![Pasted image 20240327164544](https://github.com/lm3nitro/Projects/assets/55665256/30414810-fef8-43bc-8f34-6f853194ad9d)

The attacker is conducting a port scan on an internal web server, specifically targeting port 80, by employing source IP address spoofing to conceal the origin of the attack. Furthermore, the use of random source IP addresses makes it challenging to trace the attacker's actual location. Interestingly, all the randomly generated IP addresses used in the attack have a Time-To-Live (TTL) value of 64, indicating that the attacker might be operating from within the network. You can see the TTL in the output below.
```
tcpdump -nr threat_actor.pcap 'tcp[13] $ 2!=0' or tcp[13] & 16!=0' -vvvA -c 1
```
![Pasted image 20240327165516](https://github.com/lm3nitro/Projects/assets/55665256/32ba57a4-79de-4b4c-8b3d-09a13ee98f7f)

In summary, the combination of a TCP SYN flood with a port sweep and incoming packets having a TTL value of 64 suggests a sophisticated and potentially internal attack strategy. Understanding these behaviors is crucial for devising effective defense mechanisms and mitigating the impact of such attacks on network infrastructure and services.

## Scenario 10: TCP Hijacking

TCP hijacking involves an attacker intercepting and manipulating TCP sessions to gain unauthorized access to sensitive information. The attacker exploits vulnerabilities such as predictable sequence numbers or weak authentication to impersonate legitimate parties and control the hijacked sessions, allowing for data theft or manipulation. 

![Pasted image 20240327175000](https://github.com/lm3nitro/Projects/assets/55665256/e783d2df-687b-4e13-bab9-23239141d63e)

To maintain the hijacked session, the attacker must prevent ACKs from reaching the compromised machine, achieved by either delaying or blocking the ACK packets. This tactic is often used alongside ARP poisoning, leading to observable patterns in our traffic analysis.

When looking at the pcap, we can identify a large number of ARP requests coming from one particular node when monitoring the ARP protocol.

![Pasted image 20240327200915](https://github.com/lm3nitro/Projects/assets/55665256/de3f64f9-2f34-4363-8d51-ca27913bd238)

In the screenshot below it shows all of the ARP requests send by  08:00:27:53:0c:ba to ff:ff:ff:ff:ff:ff:

![Pasted image 20240327202326](https://github.com/lm3nitro/Projects/assets/55665256/b380cb11-5a73-4c7e-b21a-a2ab71516b6b)

The node is flooding the network with ARP requests, seeking the MAC addresses corresponding to each IP node in the network.

![Pasted image 20240327202448](https://github.com/lm3nitro/Projects/assets/55665256/657d58ba-7fc6-4b7d-85eb-3dcd3d08c4c3)

In this scenario, we observe that IP address 192.168.10.5 is generating a significant number of requests to all network nodes with MAC addresses ending in 0c:ba. Subsequently, the same IP address masquerades as 192.168.10.1 with a MAC address ending in b9:4f. This allows the attacker to impersonate any node on the network or conduct a Man-in-the-Middle (MITM) attack, directing all traffic through the attacker's system.

![Pasted image 20240327181139](https://github.com/lm3nitro/Projects/assets/55665256/59bdf099-e010-4eb0-a54c-a5ec4bbff16d)

By actively monitoring the target connection they intend to hijack, the attacker conducts sequence number prediction to inject malicious packets in the correct order. During this injection, they spoof the source address to appear identical to our affected machine.

![Pasted image 20240327174332](https://github.com/lm3nitro/Projects/assets/55665256/43ccec86-983b-4465-aa93-2b2263860f25)

To detect TCP session hijacking via TCP header flags, it's crucial to actively monitor and analyze TCP traffic between hosts for any irregularities in the flags. For instance, unexpected or repetitive SYN, ACK, FIN, or RST flags may indicate an abnormal attempt to start, acknowledge, terminate, or reset a TCP connection without following the normal protocol. Similarly, discrepancies like mismatched or out-of-sequence sequence numbers could suggest packet injection or modification, while inconsistent or absent PSH or URG flags may signal attempts to tamper with data or packet priority. By vigilantly monitoring these TCP header flag anomalies, detecting potential malicious activities related to TCP session hijacking becomes feasible, allowing for effective protection against such attacks.

To safeguard against TCP/IP hijacking, consider implementing various protective measures. Start by utilizing reputable antivirus software capable of detecting and preventing packet sniffing and injection. Additionally, employ encryption technologies like VPNs or SSL/TLS to encrypt your network traffic, enhancing its security. Lastly, maintain good security practices such as regularly updating software and using strong, unique passwords to fortify your defense against potential threats.

## Metigation

To prevent TCP scanning and hijacking, implement a robust security strategy comprising firewall configurations that block unauthorized TCP traffic, intrusion detection/prevention systems for detecting and blocking malicious attempts, network segmentation with strong access controls, and strong authentication mechanisms such as multi-factor authentication and secure password policies. Additionally, employ encryption protocols like SSL/TLS for securing TCP communications, conduct regular security audits and vulnerability assessments, keep network devices and software up to date with patches, and continuously monitor network traffic and logs for anomalies. 
