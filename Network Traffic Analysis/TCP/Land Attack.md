# Land Attack

<img width="673" alt="Screenshot 2024-10-10 at 10 05 23 PM" src="https://github.com/user-attachments/assets/3fb7583c-1b23-4e4a-b288-09a456e0e5e9">

In a LAND attack, the attacker sends specially crafted packets with the same source and destination IP addresses as well as the same source and destination port numbers. This can confuse the target device's TCP/IP stack, causing it to enter a loop where it keeps processing the malicious packets, consuming CPU resources and potentially leading to a denial of service.

### How it works:

1. Network Flooding: Attackers may flood the network with excessive traffic, consuming bandwidth and overwhelming network devices such as switches and routers. This can lead to significant slowdowns or complete outages.
2. Broadcast Storms: By sending a high volume of broadcast packets, attackers can create a broadcast storm that saturates the network, causing legitimate traffic to be dropped and devices to become unresponsive.
3. Resource Exhaustion: Targeting specific devices, such as servers or network services, by sending a high volume of requests can exhaust their resources, leading to service disruptions.

### Analysis

Let's see an example below:
```
tcpdump -nr threat_actor.pcap 'tcp[13] $ 2!=0' and net 192.168.10.0/24 -c 20
```

![Pasted image 20240327163548](https://github.com/lm3nitro/Projects/assets/55665256/7bbb6ac9-69b5-4084-b351-0b4c04b54a0d)

Here we see traffic originating from the same IP address as the destination with incremental source ports. This makes it seem like the attack is coming from the target itself. When the target device receives these packets, it attempts to respond to itself. Since the device tries to establish a connection or respond to the packet it "received" from itself, it can create a feedback loop. This can lead to resource exhaustion as the device becomes overwhelmed trying to process these requests.

### Effects:

+ Resource Exhaustion: The device may become unresponsive or slow due to the excessive processing of these self-referential packets.
+ Network Disruption: Legitimate traffic can be affected as the device struggles to respond to both legitimate and malicious requests.
+ Potential Crashes: In some cases, the target system may crash or require a reboot to recover from the overload.

### Defense:

+ Use ACLs on routers and switches to prevent packets with matching source and destination IP addresses from being forwarded within the network.
+ Regularly update operating systems, applications, and network devices to ensure they are protected against known vulnerabilities. Many older systems may be more susceptible to LAND attacks due to inadequate handling of malformed packets.
+ Implement QoS policies that prioritize legitimate traffic over potential attack traffic, ensuring essential services remain available even under load.
