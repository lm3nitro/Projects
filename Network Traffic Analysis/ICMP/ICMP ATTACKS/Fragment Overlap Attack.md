# Fragment Overlap Attack

### Scenario:
In a cybersecurity monitoring center, we noticed a sudden surge in network anomalies during a routine traffic analysis. Upon closer inspection, we identified a pattern of fragmented IP packets with overlapping payloads arriving at a critical server. The unusual traffic behavior raised suspicions of a potential Tear Drop attack, known for its technique of sending malformed packets designed to confuse and disrupt systems. The attack was targeting a web server hosting a high-traffic e-commerce platform, posing a significant risk to customer data security and service availability. 

> [!NOTE]  
> A Fragment Overlap Attack, also referred to as an IP Fragmentation Attack, exploits the way IP handles data transmission and processing. These attacks, categorized as Denial of Service (DoS) attacks, involve the attacker overloading a network by exploiting datagram fragmentation mechanisms.

## Analysis

Lets take a look at that traffic observed:

```
tcpdump -nr icmp_threat_actor.pcap '((ip[6:2] > 0) and (not ip [6] = 64))' -vvvvXXA
```

![Pasted image 20240326134730](https://github.com/lm3nitro/Projects/assets/55665256/6744eff6-3ab6-4a42-bc39-259cd6a5a86b)

The initial indication of this behavior is the fragmentation of ICMP data, split into three chunks of 40 bytes each, with an overlapping fragment noted on the third packet, possibly suggesting an attempt to evade an Intrusion Detection System (IDS). An effective method for identifying overlapping fragments is to examine the last ICMP fragment packet with an offset of 80 and flags set to [none]. This indicates that host 192.168.11.13 sent three fragmented packets of 40 bytes each, while host 192.168.11.61 only received two, totaling 80 bytes.

Conclusion: We can confirm that the traffic analyzed above is indicative of a Fragment Overlap Attack/Teardrop Attack.

## Mitigation:

+ Configure IPS with specific rules to detect and block fragmented packets that exceed typical sizes or display overlapping characteristics.
+ Use tools that analyze traffic patterns and detect anomalies based on typical behavior, allowing for the identification of unusual, fragmented traffic.
+ Harden the TCP/IP stack of operating systems by applying configurations that limit the handling of fragmented packets or improve error handling.
+ Implement deep packet inspection to analyze the contents of packets beyond basic header information. DPI can help identify malformed packets or suspicious fragment patterns before they reach their destination.
