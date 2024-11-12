# TCP Xmas Scan

<img width="775" alt="Screenshot 2024-10-10 at 2 28 57 PM" src="https://github.com/user-attachments/assets/678f01b5-1fb3-4194-846b-1d1427d170f6">

A TCP Xmas scan is a type of network reconnaissance technique used to identify open ports on a target system. It gets its name from the "Xmas" tree packet, which is a TCP packet with the FIN, URG, and PSH flags set to 1. The primary goal of an Xmas scan is to identify open ports and assess the security posture of a target system. It can also help in determining the operating system of the target based on the responses received.

### How it works:

When performing an Xmas scan, the attacker sends these specially crafted packets to a range of ports on the target system.
Response Behavior:

+ Open Ports: If the port is open, the system typically does not respond to the packet.
+ Closed Ports: If the port is closed, the system responds with a TCP RST (reset) packet.
+ Filtered Ports: If a firewall or filtering device is in place, it may drop the packet without sending a response.
    
### Analysis:

In the example below, I will take a look and see the behavior described above. In this scan I can see that the host performing the scan is sending packets with the flags (FPU) set and targeting the host on all the ports:

```
tcpdump -nr threat_actor.pcap 'tcp[13] & 32!=0' or tcp[13] & 1!=0' -c 20
```

![Pasted image 20240327155225](https://github.com/lm3nitro/Projects/assets/55665256/a6434005-6b76-438c-b3a1-a6c7435cf034)

### Limitations:

+ Detection: Many modern IDS systems can detect such scans and flag them as suspicious activity.
+ Firewalls may block these packets, making the scan less effective.
+ Different operating systems may respond differently to Xmas scans, which can lead to inconsistencies in results.

### Benefits:

+ OS Fingerprinting: The way different operating systems respond to Xmas scans can provide valuable information for OS fingerprinting, helping to identify the target's operating system.
+ Port Status Identification: It effectively identifies open and closed ports on a target system, which is essential for vulnerability assessments.
+ Evasion of Basic Firewalls: Some firewalls may not inspect or block Xmas packets, making it possible to bypass simple security measures.
  
### Uses Cases:

+ Reconnaissance: Attackers may use Xmas scans as part of the reconnaissance phase to gather information about a target before launching an attack.
+ Network Auditing: Organizations can utilize Xmas scans to audit networks and understand which services are exposed, allowing for better security posture management.

### Defense:

Below are ways to defend against these types of scans:

+ Packet Filtering: Configure firewalls to block incoming TCP packets with unusual flag combinations (like those used in Xmas scans).
+ Deploy IDPS to monitor network traffic for suspicious patterns, including Xmas scans.
+ Regularly audit network security configurations and conduct vulnerability assessments to identify and remediate potential weaknesses.


