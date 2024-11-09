# TCP Fragmentation Scan

A TCP fragmentation scan is a port scanning technique that exploits vulnerabilities in the TCP/IP packet fragmentation and reassembly process. In this type of scan, the attacker sends specially crafted fragmented packets to a target system, aiming to bypass firewall rules, evade intrusion detection systems (IDS), or identify open ports.

### How It Works

In this technique, the attacker crafts TCP packets that are fragmented into smaller pieces. This can be done using tools that allow for the manipulation of TCP/IP headers. The attacker then sends these fragmented packets to various ports on the target system.
   
Response Behavior:
+ Open Ports: If a port is open, the target typically does not respond to the fragments.
+ Closed Ports: If a port is closed, the target will respond with a TCP RST (reset) packet, indicating that the port is not available.
+ Filtered Ports: If a firewall or filtering device is in place, it may drop the fragmented packets, resulting in no response.

### Analysis:

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

### Benefits:

+ Evasion of Detection: Fragmentation can help bypass security systems that are configured to block or alert on full TCP scans.
+ Some firewalls may be less effective at inspecting fragmented packets, allowing them to slip through undetected.

### Use Cases:

+ Organizations may use TCP Fragmentation scans to test the effectiveness of firewalls and intrusion detection systems.
+ Attackers may employ this technique as part of their reconnaissance efforts to identify vulnerabilities in a target network.

### Defense:

+ Create custom IPS rules that specifically identify, and block traffic patterns associated with fragmentation scans, such as multiple fragmented packets targeting different ports.
+  Set up alerts that trigger when the number of fragmented packets from a single IP address exceeds a defined threshold within a short time frame.
+  Integrate threat intelligence feeds into your security tools to stay informed about the latest scanning techniques, including fragmentation scans, and adjust your defenses accordingly.
