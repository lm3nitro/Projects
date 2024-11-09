# Ping of Death

### Scenario:

Recently, there have been reports of intermittent network disruptions and system crashes in one of your data centers. Suspecting a potential cyber-attack, your team is tasked with investigating and identifying the source of these disruptions.

## Analysis

Lets take a look at the pcap that we were provided with:

```
tcpdump -nr ping_threat_actor.pcap ip -vvvv -c 10
```

![Pasted image 20240327133805](https://github.com/lm3nitro/Projects/assets/55665256/1cc06421-1df9-421e-aa47-fcd0218ef913)

Here we can see ICMP Echo Request packets (pings) that are deliberately crafted to exceed the maximum size allowed by the IP protocol (MTU).
These oversized ICMP packets may be fragmented into smaller fragments for transmission. Lets take another look at this behavior.

```
tcpdump -nr ping_threat_actor.pcap ip and net 192.168.11.0/24
```

![Pasted image 20240327140433](https://github.com/lm3nitro/Projects/assets/55665256/6c46c6a6-d39c-4f4f-b237-f7b87d0ab818)

Now, lets take a look at the payload of one of these larger ICMP packets.

```
tcpdump -nr ping_threat_actor.pcap ip and net 192.168.11.0/24 -vvXX -c 1
```

![Pasted image 20240327140656](https://github.com/lm3nitro/Projects/assets/55665256/5d7fc950-bf1e-42ea-ae7d-6ba90d8e03e7)

Conclusion:

Based on the observeD ouputs above, I was able to see that the attacker was sending malformed ICMP Echo Request packets that exceeded the maximum allowable size. Typically, ping packets are much smaller, but the attacker crafted these packets that are fragmented to bypass restrictions. When these fragments reach the target system, it attempts to reassemble them. 
However, the reassembly process may overwhelm the system’s memory or buffer, leading to crashes or instability.

### Mitigation:

+ Ensure that all operating systems, applications, and network devices are regularly updated to include the latest security patches. Many modern systems have already fixed vulnerabilities related to oversized ICMP packets.
+ Configure firewalls to block ICMP Echo Request packets from untrusted sources or limit the size of ICMP packets.
+ Engage in periodic penetration testing to simulate attacks and assess the effectiveness of your security measures.
+ Continuously monitor network traffic for anomalies, unusual spikes in ICMP traffic, or unexpected patterns that may indicate a Ping of Death attack.
