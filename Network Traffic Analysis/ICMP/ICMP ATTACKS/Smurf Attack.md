# Smurf Attack

### Scenario
You are a network security analyst at a medium-sized enterprise. Recently, the organization experienced significant downtime and intermittent connectivity issues. You suspect a potential Distributed Denial of Service attack, specifically a Smurf attack, which exploits ICMP traffic to overwhelm network resources. Your task is to analyze the network traffic to confirm whether a Smurf attack is occurring.

> [!NOTE]  
> A Smurf Attack is a type of DDoS attack that capitalizes on Internet Protocol (IP) broadcast addresses for broadcasting numerous requests to a target IP address, originating from various sources. In this attack, the attacker dispatches a substantial quantity of ICMP echo requests to the broadcast address of a network. This action is orchestrated to give the impression that these requests stem from the target's IP address. Subsequently, these requests are disseminated to every device within the network, generating a substantial volume of traffic that has the potential to overwhelm the target's resources, leading to system overload and potential crashes.

## Analysis

Let's take a look at a pcap and see how we can identify an ICMP Smurf Attack. 

```
tcpdump -nnr icmp_threat_actor.pcap 'ip[0]==0' or 'ip[0]==8' and ! not dst net 192.168.10.0/24 -vvv -c 30
```

![Pasted image 20240325233000](https://github.com/lm3nitro/Projects/assets/55665256/57e3dd74-2036-4885-a751-6671d0e5a669)

We need to monitor our network traffic for an unusually high number of ICMP replies originating from a single host directed towards our affected host, as attackers may utilize fragmentation and payload data in these ICMP requests to inflate the traffic volume. In the screenshot above, it is difficult to show since the output is larger. However, what I was able to see is that the attacker had impersonated the IP of a letgitimate host (target) on the network.

```
tcpdump -nnr icmp_threat_actor.pcap 'ip[0]==0' or 'ip[0]==8' and ! not dst net 192.168.10.0/24 |wc -l
```

![Pasted image 20240325233127](https://github.com/lm3nitro/Projects/assets/55665256/a9623975-6891-495c-80e2-86931112cc38)

In the screenshot below which is another snippet of the pcap, I observed the internal host sending ICMP requests to the broadcast address:

![Pasted image 20240327115444](https://github.com/lm3nitro/Projects/assets/55665256/86e31a76-665f-4a77-99a4-4185194e1e37)

Conclusion:

Based on the information identified in above, we can confirm that this was a SMURF attack. 

### Mitigation

1. On routers and switches, disable IP directed broadcasts to prevent attackers from sending packets to a broadcast address, which amplifies the attack.
2. Set rate limits on ICMP traffic to control the volume of incoming requests. This helps to ensure that no single source can overwhelm the network with excessive traffic.
3. Set up firewall rules to block ICMP packets from unknown or untrusted sources, particularly those targeting broadcast addresses.
4. Conduct regular security assessments and penetration testing to identify vulnerabilities that could be exploited in a Smurf attack or other DDoS attacks.
