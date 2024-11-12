# TCP ACK Scan

A TCP ACK scan is a network scanning technique used primarily to determine the state of a firewall or packet filtering device. Unlike other scans, the ACK scan doesn't check whether ports are open but instead checks whether packets are filtered (blocked) or unfiltered (allowed through) by firewalls or other security mechanisms.

### How it works:

ACK Packet: The scanning tool sends a TCP packet with only the ACK flag set to a specific port on the target host. This mimics part of an ongoing connection without sending an initial SYN request.

Response from Target:

+ If the packet reaches the target and the port is unfiltered, the target typically responds with an RST (Reset) packet. This indicates that the ACK packet made it to the destination, but the target didn't recognize it as part of an active connection.
+ If the packet is filtered by a firewall, there will either be no response at all (the packet is silently dropped), or in some cases, an error message may be generated (depending on the firewall’s configuration).

### Analysis

In the following pcap, I see the host 192.168.10.5 sending a large amount of ACK packets to my host 192.168.10.1. These packets have some unusual traffic as they are all originating from the same source port and going to the same IP address but in different ports. I can also see the sequence number remains the same throughout all of the packets. 

```
tcpdump -nr threat_actor.pcap tcp[tcpflags] == tcp-ack and portrange 1-65365
```

![Pasted image 20240327153755](https://github.com/lm3nitro/Projects/assets/55665256/443bb5d8-effc-44b1-9ced-dbbe2fc9ab57)

When analyzing a TCP ACK scan using tcpdump, look for packets with the ACK flag set, targeting specific port ranges commonly used by the scanning device, and observe timing patterns, and response behaviors.

### Use Cases:

+ Firewall Detection: The main purpose of the ACK scan is to determine whether a firewall or filtering device is actively blocking certain packets.
+ Firewall Rules Mapping: An ACK scan helps identify which ports or IP ranges are filtered, providing information about the security policy in place.
+ Detecting Stateful Firewalls: Since stateful firewalls track the state of network connections, they allow packets that are part of an existing, legitimate connection (ACK packets), while dropping unsolicited packets like SYN or FIN.
  
### Defense:
To defend against a TCP SYN scan, the following can be done:

+ Limit Information Exposure: Configure servers to respond minimally to unsolicited traffic.
+ (IDS/IPS): Implement IDS/IPS solutions to monitor network traffic for scanning patterns, including TCP ACK scans.
+ Firewalls: Ensure that firewall is set up to reject unsolicited ACK packets on unused or unnecessary ports. 
