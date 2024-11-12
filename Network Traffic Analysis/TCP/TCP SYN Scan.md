# TCP SYN Scan

<img width="689" alt="Screenshot 2024-10-09 at 11 20 28 PM" src="https://github.com/user-attachments/assets/3dbb0a53-e6b8-4b6e-bcb6-9874bd012a04">

A TCP SYN scan is a common and efficient network scanning technique used to detect open ports on a target system. This method is also called a half-open scan because it doesn't complete the full TCP handshake, reducing the likelihood of detection.

### How it works:

The scanning tool sends a TCP packet with the SYN flag set to a specific port on the target host, indicating an attempt to start a connection.
Response from Target:

+ If the port is open, the target host replies with a SYN-ACK packet, indicating it is ready to complete the connection.
+ If the port is closed, the target responds with a RST (Reset) packet, indicating no service is listening on that port.
+ If the port is filtered (firewall), there might be no response, or the packet is dropped, depending on the firewall rules.

Abort Connection: Upon receiving a SYN-ACK response from an open port, the scanning host immediately sends an RST packet, effectively aborting the connection before it completes the handshake. This is why it's called a "half-open" scan—it doesn’t complete the three-way handshake of TCP (SYN, SYN-ACK, ACK), which would establish a full connection.

### Analysis

Let's look at the following pcap and the abnormal TCP behavior. To start, I will Filter for tcp syn flags and ports (1-1024):

```
tcpdump -nr threat_actor.pcap tcp[tcpflags] == tcp-syn and portrange 1-1024
```

![Pasted image 20240325120655](https://github.com/lm3nitro/Projects/assets/55665256/5faa423d-3c40-4433-b7e0-9b5b151f057c)

It appears that the threat actor located at 192.168.11.62 is actively scanning the node 192.168.11.46 in search of open ports. I observed a significant number of SYN (synchronize) requests being sent to 192.168.11.46 across multiple ports within a fraction of a second.

To pinpoint the extent of the synchronization requests sent by the threat actor, I need to narrow down my analysis to determine the exact number of SYN requests received by 192.168.11.46.

```
tcpdump -nr threat_actor.pcap src host 192.168.11.62 and dst host 192.168.11.62 and dst host 192.168.11.46 and tcp[tcpflags] == tcp-syn | wc -l
```

![Pasted image 20240325122547](https://github.com/lm3nitro/Projects/assets/55665256/0454d367-56d4-49c1-bd12-5f51240e92fa)

### Benefits of a TCP SYN Scan:

+ Stealthy: Since it doesn't complete the TCP handshake, it's less likely to be logged or detected by an IDS or firewalls, compared to other types of scans that establish full connections.
+ Fast: A SYN scan is relatively quick, making it effective for scanning large networks.
+ Informative: It can accurately detect open, closed, and filtered ports, giving insight into the services and applications running on the target.

### Use Cases:

+ Penetration Testing: SYN scans are used to identify network vulnerabilities by discovering which ports and services are exposed to the internet.
+ Network Security Audits: Organizations use this method to verify that only authorized services are running and accessible on their networks.

### Defense:

To defend against a TCP SYN scan, the following can be done:

+ Configure firewalls to block or filter incoming traffic from untrusted sources, especially on unused or unnecessary ports. A firewall can be set to drop unsolicited SYN packets, making the scan less effective.
+ Deploy IDS/IPS tools (like Snort or Suricata) to monitor for unusual patterns, such as rapid or excessive SYN requests from a single source.
+ Restrict access to sensitive or critical systems by using network segmentation and placing important assets in isolated networks or VLANs.
