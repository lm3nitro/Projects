# TCP Reset Scan

A TCP Reset scan is a network reconnaissance technique used to determine the state of ports on a target system by sending TCP packets with the RST (reset) flag set. This scan method helps identify open and closed ports without establishing a full TCP connection. If an adversary aims to disrupt our network with a denial-of-service attack, they might use a TCP RST Packet injection attack, also known as TCP connection termination. This attack involves spoofing the source address to the affected machine's IP, modifying TCP packets with the RST flag to terminate connections, and targeting ports currently in use by the target machines.

### How it works:

In this type of scan, TCP packets are sent with the RST flag set to various ports on the target system.

Response Behavior:

+ Open Ports: If the port is open, the target typically does not respond to the RST packet.
+ Closed Ports: If the port is closed, the target responds with a TCP RST packet, indicating that the connection could not be established.
+ Filtered Ports: If a firewall or filtering device is in place, it may drop the RST packet without any response.

### Analysis:

Below is an example:

```
tcpdump -nr 'tcp[13] & 4!=0' && ! net 10.0.0.0/8
```
![Pasted image 20240327161657](https://github.com/lm3nitro/Projects/assets/55665256/404ceed5-b5a4-4de2-8c07-833817cddf31)

This attack involves several components:

+ The attacker spoofs the source address to match the affected machine's.
+ The attacker alters the TCP packet to include the RST flag, ending the connection.
+ The attacker designates the destination port to match one currently utilized by one of our machines.

One way to identify a TCP RST attack is by comparing the physical address of the transmitter with our network device list; a mismatch, such as a different MAC address, indicates potential malicious activity. However, attackers can spoof MAC addresses to evade detection, leading to retransmissions and other anomalies similar to those seen in ARP poisoning incidents.

### Benefits:

+ Reset scans can be faster than traditional connection-based scans since they do not require a three-way handshake to establish a connection.
+ Some firewalls may not inspect RST packets as thoroughly as others, allowing the scan to go undetected.
  
### Use Cases:

+ Testing: TCP RST scans can be used to evaluate how well firewalls or intrusion detection systems respond to non-standard scanning methods, identifying gaps in defenses.
+ Attackers may deploy RST scans to map out open and closed ports on a target system as part of their reconnaissance phase, collecting intelligence for future exploitation.
  
### Defense:

+ One way to identify a TCP RST attack is by comparing the physical address of the transmitter with our network device list; a mismatch, such as a different MAC address, indicates potential malicious activity. However, attackers can spoof MAC addresses to evade detection, leading to retransmissions and other anomalies similar to those seen in ARP poisoning incidents.
+ Use anomaly detection to identify unusual traffic patterns that deviate from normal behavior, such as excessive RST packets.
+ Implement flow monitoring solutions to track the volume and types of packets passing through your network. An unusual spike in RST packets can indicate a scan.
+ Set up alerts for instances where a single IP address attempts to access multiple ports in a short period, especially with RST responses.
