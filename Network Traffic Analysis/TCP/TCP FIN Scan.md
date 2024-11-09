# TCP FIN Scan

<img width="794" alt="Screenshot 2024-10-09 at 11 33 38 PM" src="https://github.com/user-attachments/assets/887ebd25-cf08-4350-b170-f37654b2661f">

A TCP FIN scan is a stealthy network scanning technique used to determine the state of ports on a target system. This method involves sending TCP packets with only the FIN flag set, which is typically used to indicate the end of a TCP connection. 

### How it works:

The scanning tool sends a TCP packet with the FIN flag set to various ports on the target host. This packet is sent without a preceding SYN packet, which is part of the normal TCP handshake.

Response from the Target:

+ If the target port is closed, it will respond with an RST (Reset) packet. This indicates that there is no service listening on that port.
+ If the port is open, many operating systems (especially UNIX-based systems) will not respond at all. This lack of response suggests that the port is open, as it does not consider the unsolicited FIN packet a legitimate request.

### Analysis:

Let's look at an example of this type of scan in the pcap below:

```
tcpdump -nr threat_actor.pcap tcp[tcpflags] == tcp-fin -c 20
```

![Pasted image 20240327154309](https://github.com/lm3nitro/Projects/assets/55665256/1895a7b0-0a00-432a-badb-3eae4205eecf)

Overall, TCP FIN scans are a reconnaissance tool used by threat actors to gather information about a target system's port states, network configuration, and potential vulnerabilities while attempting to remain undetected and evade security measures.

### Use cases:
Threat actors might use a TCP FIN scan for several reasons:

+ Stealthy Scanning
+ Port State Identification
+ Firewall Evasion
+ Operating System Fingerprinting
+ Anomaly Detection Testing

Network Security Assessments: Organizations may use FIN scans to evaluate their own security posture by detecting which ports can be identified as open or closed by potential attackers.

### Defense:

The following can be done to defend against this type of scans:

+ Set up firewalls to filter out unsolicited FIN packets on unused or unnecessary ports.
+ Modify the system's TCP/IP stack behavior to handle FIN packets differently. This can involve configuring the system to ignore or respond with a specific pattern to unsolicited FIN packets, thereby obfuscating port states.
+ Use Port Knocking: This technique involves keeping ports closed until a specific sequence of packets (the "knock") is sent to the system.
