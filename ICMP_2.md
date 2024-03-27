

![Pasted image 20240325233711](https://github.com/lm3nitro/Projects/assets/55665256/f74024b1-c25b-4779-b500-6b5f929b5332)


Step 1:
Note: In this pcap we have 8330 packets we will need to filter to find out the abnormal packets. 
 
 Filter for ICMP number of  packets :



Identifying  IP Source & Destination Spoofing Attacks:

Let's consider the following when analyzing these fields for our traffic analysis efforts.

The Source IP Address should always be from our subnet - If we notice that an incoming packet has an IP source from outside of our local area network, this can be an indicator of packet crafting.

The Source IP for outgoing traffic should always be from our subnet - If the source IP is from a different IP range than our own local area network, this can be an indicator of malicious traffic that is originating from inside our network.

An attacker might conduct these packet crafting attacks towards the source and destination IP addresses for many different reasons or desired outcomes. Here are a few that we can look for:

Decoy Scanning:
Random Source Attack DDoS 
SMURF Attacks
Duplicate Fragments



There are 15 different types of ICMP messages and some are categorized as error reporting and query.

A few popular ICMP messages:

Error Reporting Query
Type Message Type Message
3 Destination Unreachable 8/0 Echo(request/reply)
4 Source quench 13/14 Timestamp(req/Reply)
11 Time exceeded 18/18 Address mask(req/rep)
12 Parameter problem 10-Sep Router advertisement
5 Redirection

## Scenario 2

As a security analyst, I was tasked with investigating a peculiar Packet Capture (PCAP) file that had been flagged due to unusual ICMP traffic patterns. The PCAP had been captured from a network segment that was typically quiet, used mainly for administrative purposes and internal communications.

Upon starting our investifgation, we see the host 192.168.10.5 responding to a random source addresses, and notice that the payload is larger than the normal ICMP traffic (64 bytes). 

>#### One factor to bear in mind is that attackers might attempt to fragment their traffic to avoid detection and to impose greater resource strain on the victim host.

```
tcpdump -nnr icmp_threat_actor.pcap "icmp[0]==0" and ! not src net 192.168.10.0/24 -c 10
```

![Pasted image 20240325215127](https://github.com/lm3nitro/Projects/assets/55665256/2a1a793e-7a85-4578-b9b4-e0d8ca9c7dd1)

Let's dig a little deeper to see what we can find...
```
tcpdump -nnr icmp_threat_actor.pcap "icmp[0]==0" and ! not src net 192.168.10.0/24 -c 10 -XXA -vvv -c 1
```
![Pasted image 20240325224130](https://github.com/lm3nitro/Projects/assets/55665256/b6036d71-520f-4cc4-a977-716d985c61cb)

In this observation, we note that the threat actor is exploiting the ICMP protocol by augmenting the payload size and fragmenting the traffic, aiming to transmit a substantial amount of data to this specific node. This tactic could be utilized to inundate the server with spurious traffic, potentially leading to service disruption. Additionally, the Time-to-Live (TTL) value indicates that this traffic appears to originate from an internal node, further masking the attacker's identity and intentions.

Next, lets take a look at the the amount of framagments going to and from 192.168.10.5.
```
tcpdump -nnr icmp_threat_actor.pcap 'ip[6:2] > 0' and host 192.168.10.5 | wc -l
```

![Pasted image 20240325230140](https://github.com/lm3nitro/Projects/assets/55665256/2a53c996-5fa4-448f-b89b-c21b2cb9e8a5)

Let's select one conversation to further analyse it and see what is really going on! 
```
tcpdump -nnr icmp_threat_actor.pcap host 111.43.91.100 -vv
```

![Pasted image 20240325224948](https://github.com/lm3nitro/Projects/assets/55665256/21865b21-ddea-4b62-8927-f3d32e126100)

The receiving node is being bombarded with a significant volume of fragmented ICMP random spoof requests from an internal source. This activity has the potential to cause a denial of service (DoS) on node 192.168.10.5.

## Scenario 3

Let's take a look at a pcap and see how we can identify a ICMP Smurf Attack. An ICMP Smurf attack floods a target network with ICMP Echo Request packets, using spoofed source addresses to amplify the volume of traffic and overwhelm the victim's resources.
```
tcpdump -nnr icmp_threat_actor.pcap 'ip[0]==0' or 'ip[0]==8' and ! not dst net 192.168.10.0/24 -vvv -c 30
```
![Pasted image 20240325233000](https://github.com/lm3nitro/Projects/assets/55665256/57e3dd74-2036-4885-a751-6671d0e5a669)

One key indicator to monitor in our network traffic is an unusually high number of ICMP replies originating from a single host directed towards our affected host, as attackers may utilize fragmentation and payload data in these ICMP requests to inflate the traffic volume.
```
tcpdump -nnr icmp_threat_actor.pcap 'ip[0]==0' or 'ip[0]==8' and ! not dst net 192.168.10.0/24 |wc -l
```

![Pasted image 20240325233127](https://github.com/lm3nitro/Projects/assets/55665256/a9623975-6891-495c-80e2-86931112cc38)

Smurf Attacks are a significant form of distributed denial-of-service attack, characterized by their method of leveraging random hosts to ping the victim host. Essentially, an attacker executes these attacks as follows:

1. The attacker sends an ICMP request to active hosts, using a spoofed address that appears to be the victim host.
2. The active hosts, believing the request is from the legitimate victim host, respond with ICMP replies.
3. This activity can lead to resource exhaustion on the victim host, impacting its ability to function normally.


################
Identify overlapping / duplicate fragments
**Teardrop**: 



![Pasted image 20240326134730](https://github.com/lm3nitro/Projects/assets/55665256/6744eff6-3ab6-4a42-bc39-259cd6a5a86b)


The first indicator of this behaviour is the fragmentation of the data on the ICMP that are split in 3 chunks of 40 bytes each, also the overlaying fragment on the third packet, that could indicate some on the network is train to bypass an IDS. One way to identify  overlapping fragments is to see the last ICMP fragment packet that has offset 80, flags [none] This mean that the host 192.168.11.13 has sent 3 fragmented packets  of 40 bytes and host 192.168.11.61 only received 2 with has 80 bytes total. 

A Fragment Overlap Attack, also known as an IP Fragmentation Attack, is an attack that is based on how the Internet Protocol (IP) requires data to be transmitted and processed. These attacks are a form of Denial of Service (DoS) attack where the attacker overloads a network by exploiting datagram fragmentation mechanisms. 


Identify  suspicions host using traceroute on the network:


![Pasted image 20240326151132](https://github.com/lm3nitro/Projects/assets/55665256/2f5bb032-63fb-41d4-b800-38a5f9e4ec0c)


![Pasted image 20240326151607](https://github.com/lm3nitro/Projects/assets/55665256/6ec1ea9b-efd1-414e-b241-52ed9bb85332)



Threat actor usually try to map the network by using tools like traceroute.  Something to look for is the low TTL from  the source, low packet size and the high amount the ICMP use.  The victim host that is being is sending it's real TTL.

Identify: 
ICMP  spoof  same source and destination with incremental fragmentation

![Pasted image 20240326155509](https://github.com/lm3nitro/Projects/assets/55665256/b1912aaf-1906-4f18-8ab0-4d4a6ec36e75)

