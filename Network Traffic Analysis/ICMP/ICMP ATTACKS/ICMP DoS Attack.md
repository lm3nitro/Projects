#  ICMP DoS Attack

### Scenario:
You are a network security analyst at a cybersecurity firm. Recently, your team received a suspicious report from a client about unusual network activity, potentially indicating a data exfiltration attempt using ICMP packets. The PCAP had been captured from a network segment that was typically quiet, used mainly for administrative purposes and internal communications. Your task is to analyze a provided PCAP (Packet Capture) file to identify and investigate what is happening. 

## Analysis

First, lets take a look at some information about the pcap:

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

Next, let'ss take a look at the the amount of fragments going to and from 192.168.10.5:

```
tcpdump -nnr icmp_threat_actor.pcap 'ip[6:2] > 0' and host 192.168.10.5 | wc -l
```

![Pasted image 20240325230140](https://github.com/lm3nitro/Projects/assets/55665256/2a53c996-5fa4-448f-b89b-c21b2cb9e8a5)

Let's select one conversation to further analyze it and see what is really going on:
```
tcpdump -nnr icmp_threat_actor.pcap host 111.43.91.100 -vv
```

![Pasted image 20240325224948](https://github.com/lm3nitro/Projects/assets/55665256/21865b21-ddea-4b62-8927-f3d32e126100)

Conclusion:

Based on the output, we see that the receiving node is being bombarded with a significant volume of fragmented ICMP random spoof requests from an internal source. This activity has the potential to cause a denial of service (DoS) on node 192.168.10.5.

### Mitigation Strategies

To address this issue and mitigate similar threats in the future, the following steps can be taken:

+ Implement access control lists (ACLs) on routers and firewalls to limit ICMP traffic, allowing only legitimate sources and types of ICMP packets.
+ Configure rate limiting for ICMP requests to prevent excessive traffic from overwhelming any single node.
+ Deploy IDS solutions that can identify and alert on anomalous ICMP traffic patterns, enabling prompt investigation and response.
+ Isolate sensitive nodes within their own network segments to limit the impact of potential DoS attacks originating from internal sources.
+ Continuously monitor network traffic for unusual spikes in ICMP activity and conduct regular audits to detect and remediate potential vulnerabilities.
