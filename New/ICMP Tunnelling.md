# ICMP Tunnelling

ICMP tunneling is a technique that uses the Internet Control Message Protocol (ICMP) to encapsulate and transmit data packets over a network. ICMP is primarily used for sending error messages and operational information regarding network conditions, but it can be repurposed to create a tunnel for other types of traffic.

### Scenario:

In a corporate environment, Suricata is continuously monitoring traffic patterns to detect unusual activities. One day, Suricata identifies excessive ICMP traffic originating from a specific workstation and triggers an alert. The alert indicates that the workstation has sent/received a significantly higher amount of ICMP traffic than normal. Following this alert, a pcap file is generated to provide a detailed record of the network traffic during the incident. You are assigned to investigate this unusual behavior. The task involves analyzing the PCAP data to uncover the source of the excessive ICMP traffic and determining whether this activity poses a security threat. 

### Tools and Technology:

Wireshark, Tshark, and Linux

## PCAP Analysis:

To get started, I opened the pcap using Wireshark and filtered the ICMP traffic. Looking at the requests and replies, they have different payloads:

![Pasted image 20240912122001](https://github.com/user-attachments/assets/bf9b27fb-db64-4867-be88-45618bc96421)

I also noted the directory that the payload was referencing and that the length varied greatly. Based on what I have observed, this is not normal ICMP traffic. The presence of ICMP packets with directory references may indicate attempts to exfiltrate data or establish a communication channel with a remote server. Attackers sometimes use ICMP as a covert communication method.

![Pasted image 20240912122259](https://github.com/user-attachments/assets/3b980dc9-7eea-482c-a03d-fa501b483c16)

Another anomaly observed in the pcap is the TTL, as seen in the screenshot below it is 246. Based on the TTL which is unusual can signify that these packets are crafted/modified:

![Pasted image 20240912123801](https://github.com/user-attachments/assets/ad13484a-6373-4616-afce-f8304740d72d)

## Tshark

![Pasted image 20241008105755](https://github.com/user-attachments/assets/6778fbe1-abc9-4775-ae56-9125bb9bf907)

Next, I decoded the ICMP payload with Tshark and extracted the payload in hex:

```
tshark -n -r weird-ping.pcap -T fields -e "data.data" -Y data.data
```

![Pasted image 20240912123437](https://github.com/user-attachments/assets/819d9525-1a8b-4272-b1ea-357350ee3823)

I then converted the HEX using XXD:

```
tshark -n -r weird-ping.pcap -T fields -e "data.data" -Y data.data | xxd -r -p | less
```

![Pasted image 20240912123315](https://github.com/user-attachments/assets/9eaedd7d-879e-49f5-833a-0c6bca0d620d)

### Summary

After analyzing the PCAP file, the following conclusions were drawn regarding the presence of ICMP tunneling:

+ Identification of ICMP Tunneling: Abnormal ICMP traffic patterns were observed, including unusually high volumes of ICMP packets with specific payload structures that referenced directories or contained encoded data.
+ The TTL values were consistent with those typically used in tunneling scenarios, and the packet sizes were irregular, deviating from standard ICMP echo requests and replies.

The analysis indicated that the traffic likely represented data being exfiltrated from the network. ICMP tunneling can be used to bypass traditional security controls since ICMP is often less scrutinized. The presence of consistent ICMP traffic with similar payloads suggested an established communication channel between the compromised host and an external entity, likely a command and control (C2) server.

Mitigation:

+ Configure firewalls to restrict ICMP traffic where feasible.
+ Implement network segmentation to limit the lateral movement of threats.
+ Utilize advanced intrusion detection systems (IDS) to monitor ICMP traffic for anomalies.
+ Implement rate limiting on ICMP traffic to restrict the number of ICMP packets that can be sent or received in a given timeframe. This helps prevent abuse and tunneling attempts.
