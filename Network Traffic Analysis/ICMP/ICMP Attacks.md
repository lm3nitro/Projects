# Identifying IP Source & Destination Spoofing Attacks

Here I will cover a series of different scenarios sorrounding ICMP attacks. Let's consider the following when analyzing these fields for our traffic analysis efforts.

1. Source IP Address Validation: Incoming packets should originate from our subnet. An IP source from outside our local area network could indicate packet crafting.

2. Outgoing Traffic Source IP Validation: The source IP for outgoing traffic should align with our subnet. Any deviation may signal malicious traffic originating from within our network.

Attackers may employ packet crafting techniques targeting source and destination IP addresses for various purposes. Some common attack scenarios to watch for include: 

+ Decoy Scanning
+ Random Source Attack DDoS 
+ SMURF Attacks
+ Duplicate Fragments

Additionally, understanding the types of ICMP messages is essential, as there are 15 distinct types categorized into error reporting and query functions. Among the notable ICMP messages are:

Error Reporting:

+ Destination Unreachable (Type 3)
+ Source Quench (Type 4)
+ Time Exceeded (Type 11)
+ Parameter Problem (Type 12)

Query:

+ Echo (Request/Reply) (Type 8/0)
+ Timestamp (Request/Reply) (Type 13/14)
+ Address Mask (Request/Reply) (Type 18/18)
+ Router Advertisement (Type 10/9)
+ Redirection (Type 5)

Lets take a look at the scenarios below. 

## Scenario 4: Traceroute Anomoly

In this scenario, I will take a look to see how wcan identify a suspicious host using tcproute in our network.

```
tcpdump -nr icmp_threat_actor.pcap 'ip[8] < 5' -vv
```
![Pasted image 20240326151132](https://github.com/lm3nitro/Projects/assets/55665256/2f5bb032-63fb-41d4-b800-38a5f9e4ec0c)

Here we can see that the TTL is 1 and the length is 8. Very unusual for ICMP traffic. Lets keep looking and see what else we can find. 

```
tcpdump -nr icmp_threat_actor.pcap host 192.168.194.149 and icmp -vv
```

![Pasted image 20240326151607](https://github.com/lm3nitro/Projects/assets/55665256/6ec1ea9b-efd1-414e-b241-52ed9bb85332)

Threat actors often attempt to map out a network using tools like traceroute. One indicator to watch for includes a low Time-to-Live (TTL) value from the source, coupled with a small packet size and a significant increase in ICMP usage. Notably, the victim host maintains its actual TTL value during this probing.

## Scenario 5: ICMP Spoof Attack

Identify: 
ICMP spoof same source and destination with incremental fragmentation

![Pasted image 20240326155509](https://github.com/lm3nitro/Projects/assets/55665256/b1912aaf-1906-4f18-8ab0-4d4a6ec36e75)


## Scenario 7: ICMP Tunelling

Threat actors may utilize ICMP tunneling for information extraction in sophisticated cyber attacks targeting organizations with valuable data assets. Vigilant monitoring, robust detection mechanisms, and proactive response strategies are essential in mitigating the risks posed by such covert communication channels. Here we can see an example of this in our pcap.

```
tcpdump -nr icmp_threat_actor.pcap icmp and not src net 192.168.0.0/16 -c1 -vvvA
```

![Pasted image 20240327152345](https://github.com/lm3nitro/Projects/assets/55665256/c675b055-d525-423d-b2f0-5abea3fe6f6d)
