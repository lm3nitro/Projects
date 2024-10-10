# TCP Null Scan

<img width="734" alt="Screenshot 2024-10-10 at 3 41 06 PM" src="https://github.com/user-attachments/assets/257f7da2-3ec2-4fda-a6b6-aa8e9c5e6af3">

A TCP Null scan is another technique used for network reconnaissance to determine the status of ports on a target system. In this type of scan, the attacker sends TCP packets with no flags set, essentially creating a "null" packet.

### How it works:

The attacker sends a TCP packet to a target port with no flags (i.e., SYN, ACK, FIN, URG, or PSH are all set to 0).

Response Behavior:
+ Open Ports: Typically, if the port is open, the target will not respond.
+ Closed Ports: If the port is closed, the target usually responds with a TCP RST (reset) packet.
+ Filtered Ports: If a firewall or filtering mechanism is in place, it may drop the packet without sending any response.

### Analysis:

Lets take a look to see what this looks like in a pcap and how we can identify it. 

```
tcpdump -nr threat_actor.pcap ! 'tcp[13] & 32!=0' or 'tcp[13] & 1!=0' -c 10
```

![Pasted image 20240327155836](https://github.com/lm3nitro/Projects/assets/55665256/87b22a4a-5311-48c2-b4ce-18f3005eba0c)

Unlike other port scanning techniques that may set specific flags to elicit responses from the target system, a null scan with flags set to none deliberately omits any flag information, creating a "null" or empty TCP packet.

### Benefits:
+ Limited Footprint: The low-key nature of the Null scan can be advantageous in environments where extensive logging or monitoring is in place.
+ Stealth: Since the Null scan does not set any TCP flags, it can be less likely to trigger alarms in some intrusion detection systems, making it a stealthy reconnaissance option.
+ It can effectively identify open and closed ports on a target system without sending conventional signals that could raise suspicion.

### Use cases:

+ Gathering Information: Attackers may use Null scans during the reconnaissance phase of an attack to collect information about the target’s network and determine the presence of services running on various ports.
+ Identifying Potential Weaknesses: Organizations can conduct Null scans as part of vulnerability assessments to identify exposed ports and services that may be vulnerable to exploitation.
  
### Defense:

Below are some options to defend against TCP Null Scans:

+ Set up firewalls to block incoming TCP packets with no flags set. This helps prevent Null scan packets from reaching their targets.
+ Deploy IDS/IPS systems to detect unusual traffic patterns associated with Null scans.
+ Implement rate limiting on your network to restrict the number of connection attempts from a single IP address. This can help mitigate the impact of scanning activities.
