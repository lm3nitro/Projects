# Identifying IP Source & Destination Spoofing Attacks

Here we will cover a series of different scenarios sorrounding ICMP attacks. Let's consider the following when analyzing these fields for our traffic analysis efforts.

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

## Scenario 1: ICMP DDOS Attack

As a security analyst, I was tasked with investigating a peculiar Packet Capture (PCAP) file that had been flagged due to unusual ICMP traffic patterns. The PCAP had been captured from a network segment that was typically quiet, used mainly for administrative purposes and internal communications.

First, lets take a look at some information about our pcap. 

```
capinfos icmp_threat_actor.pcap
```
![Pasted image 20240325233711](https://github.com/lm3nitro/Projects/assets/55665256/f74024b1-c25b-4779-b500-6b5f929b5332)

In this pcap we have 8330 packets. We will need to filter further in order get more information and see if there is any abnormal activity. Below we see the host 192.168.10.5 is responding to a group of random source addresses. We can also see that the payload is larger than the normal ICMP traffic (64 bytes). 

```
tcpdump -nnr icmp_threat_actor.pcap "icmp[0]==0" and ! not src net 192.168.10.0/24 -c 10
```

![Pasted image 20240325215127](https://github.com/lm3nitro/Projects/assets/55665256/2a1a793e-7a85-4578-b9b4-e0d8ca9c7dd1)

>#### One factor to bear in mind is that attackers might attempt to fragment their traffic to avoid detection and to impose greater resource strain on the victim host.

Let's dig a little deeper to see what we can find...
```
tcpdump -nnr icmp_threat_actor.pcap "icmp[0]==0" and ! not src net 192.168.10.0/24 -c 10 -XXA -vvv -c 1
```
![Pasted image 20240325224130](https://github.com/lm3nitro/Projects/assets/55665256/b6036d71-520f-4cc4-a977-716d985c61cb)

In this observation, we note that the threat actor is exploiting the ICMP protocol by augmenting the payload size and fragmenting the traffic, aiming to transmit a substantial amount of data to this specific node. This tactic could be utilized to inundate the server with factitious traffic, potentially leading to service disruption. Additionally, the Time-to-Live (TTL) value indicates that this traffic appears to originate from an internal node, further masking the attacker's identity and intentions.

Next, lets take a look at the the amount of fragments going to and from 192.168.10.5:
```
tcpdump -nnr icmp_threat_actor.pcap 'ip[6:2] > 0' and host 192.168.10.5 | wc -l
```

![Pasted image 20240325230140](https://github.com/lm3nitro/Projects/assets/55665256/2a53c996-5fa4-448f-b89b-c21b2cb9e8a5)

Let's select one conversation to further analyze it and see what is really going on:
```
tcpdump -nnr icmp_threat_actor.pcap host 111.43.91.100 -vv
```

![Pasted image 20240325224948](https://github.com/lm3nitro/Projects/assets/55665256/21865b21-ddea-4b62-8927-f3d32e126100)

Based on the output, we see that the receiving node is being bombarded with a significant volume of fragmented ICMP random spoof requests from an internal source. This activity has the potential to cause a denial of service (DoS) on node 192.168.10.5.

## Scenario 2: Smurf Attack

A Smurf Attack is a type of DDoS attack that capitalizes on Internet Protocol (IP) broadcast addresses for broadcasting numerous requests to a target IP address, originating from various sources. In this attack, the assailant dispatches a substantial quantity of Internet Control Message Protocol (ICMP) echo requests (commonly known as "pings") to the broadcast address of a network. This action is orchestrated to give the impression that these requests stem from the target's IP address. Subsequently, these requests are disseminated to every device within the network, generating a substantial volume of traffic that has the potential to overwhelm the target's resources, leading to system overload and potential crashes.

Let's take a look at a pcap and see how we can identify an ICMP Smurf Attack. 
```
tcpdump -nnr icmp_threat_actor.pcap 'ip[0]==0' or 'ip[0]==8' and ! not dst net 192.168.10.0/24 -vvv -c 30
```
![Pasted image 20240325233000](https://github.com/lm3nitro/Projects/assets/55665256/57e3dd74-2036-4885-a751-6671d0e5a669)

We need to monitor our network traffic for an unusually high number of ICMP replies originating from a single host directed towards our affected host, as attackers may utilize fragmentation and payload data in these ICMP requests to inflate the traffic volume.
```
tcpdump -nnr icmp_threat_actor.pcap 'ip[0]==0' or 'ip[0]==8' and ! not dst net 192.168.10.0/24 |wc -l
```

![Pasted image 20240325233127](https://github.com/lm3nitro/Projects/assets/55665256/a9623975-6891-495c-80e2-86931112cc38)

We may also  observe numerous hosts sending ping requests to our single host, which exemplifies the fundamental characteristic of Smurf attacks.

![Pasted image 20240327115444](https://github.com/lm3nitro/Projects/assets/55665256/86e31a76-665f-4a77-99a4-4185194e1e37)


## Scenario 3: Fragment Overlap Attack

In a cybersecurity monitoring center, we noticed a sudden surge in network anomalies during a routine traffic analysis. Upon closer inspection, we identified a pattern of fragmented IP packets with overlapping payloads arriving at a critical server.

The unusual traffic behavior raised suspicions of a potential Tear Drop attack, known for its technique of sending malformed packets designed to confuse and disrupt systems. The attack was targeting a web server hosting a high-traffic e-commerce platform, posing a significant risk to customer data security and service availability. Lets take a look at that traffic observed. 

```
tcpdump -nr icmp_threat_actor.pcap '((ip[6:2] > 0) and (not ip [6] = 64))' -vvvvXXA
```

![Pasted image 20240326134730](https://github.com/lm3nitro/Projects/assets/55665256/6744eff6-3ab6-4a42-bc39-259cd6a5a86b)

The initial indication of this behavior is the fragmentation of ICMP data, split into three chunks of 40 bytes each, with an overlapping fragment noted on the third packet, possibly suggesting an attempt to evade an Intrusion Detection System (IDS). An effective method for identifying overlapping fragments is to examine the last ICMP fragment packet with an offset of 80 and flags set to [none]. This indicates that host 192.168.11.13 sent three fragmented packets of 40 bytes each, while host 192.168.11.61 only received two, totaling 80 bytes.

A Fragment Overlap Attack, also referred to as an IP Fragmentation Attack, exploits the way Internet Protocol (IP) handles data transmission and processing. These attacks, categorized as Denial of Service (DoS) attacks, involve the attacker overloading a network by exploiting datagram fragmentation mechanisms.
 

## Scenario 4: Traceroute Anomoly

In this scenario, we will take a look to see how we can identiry a suspicious host using tcproute in our network.

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

## Scenario 6: Ping of Death

Recently, there have been reports of intermittent network disruptions and system crashes in one of your data centers. Suspecting a potential cyber attack, your team is tasked with investigating and identifying the source of these disruptions. Lets take a look at the pcap that we were provided with.

```
tcpdump -nr ping_threat_actor.pcap ip -vvvv -c 10
```

![Pasted image 20240327133805](https://github.com/lm3nitro/Projects/assets/55665256/1cc06421-1df9-421e-aa47-fcd0218ef913)

Here we can see ICMP Echo Request packets (pings) that are deliberately crafted to exceed the maximum size allowed by the IP protocol (MTU).
These oversized ICMP packets may be fragmented into smaller fragments for transmission. Lets take a nother look at this behavior.

```
tcpdump -nr ping_threat_actor.pcap ip and net 192.168.11.0/24
```

![Pasted image 20240327140433](https://github.com/lm3nitro/Projects/assets/55665256/6c46c6a6-d39c-4f4f-b237-f7b87d0ab818)

Now, lets take a look at the payload of one of these larger ICMP packets.

```
tcpdump -nr ping_threat_actor.pcap ip and net 192.168.11.0/24 -vvXX -c 1
```

![Pasted image 20240327140656](https://github.com/lm3nitro/Projects/assets/55665256/5d7fc950-bf1e-42ea-ae7d-6ba90d8e03e7)


## Scenario 7: ICMP Tunelling

Threat actors may utilize ICMP tunneling for information extraction in sophisticated cyber attacks targeting organizations with valuable data assets. Vigilant monitoring, robust detection mechanisms, and proactive response strategies are essential in mitigating the risks posed by such covert communication channels. Here we can see an example of this in our pcap.

```
tcpdump -nr icmp_threat_actor.pcap icmp and not src net 192.168.0.0/16 -c1 -vvvA
```

![Pasted image 20240327152345](https://github.com/lm3nitro/Projects/assets/55665256/c675b055-d525-423d-b2f0-5abea3fe6f6d)