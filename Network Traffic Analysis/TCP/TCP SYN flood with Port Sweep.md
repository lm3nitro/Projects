#  TCP SYN flood with Port Sweep

A TCP SYN flood with port sweep is a type of denial-of-service (DoS) attack that combines two techniques: SYN flooding and port sweeping. This method is designed to overwhelm a target system by consuming its resources, leading to service disruption.
If the source ports in a TCP SYN flood attack with random spoofed addresses are incremented in value, it adds another layer of complexity to the attack and is often referred to as a "port sweep" or "port scan" within the context of the SYN flood. In this scenario, the attacker not only floods the target with a large number of TCP SYN packets but also varies the source ports in an incremental manner.

### How it works:

There can be some variations on how this is carried out. In the example below, we will see the following:

1. SYN Flood Component:

    The attacker sends a barrage of TCP SYN packets to a specific target IP address and port (ex. port 80 for HTTP).
    Each SYN packet comes from a different, randomly chosen source IP address (often referred to as IP spoofing).

2. Port Sweep Component:

    The attacker targets multiple ports on the same IP address, sending SYN packets to different ports (e.g., 80, 443, 21) but still maintaining a pool of different source IP addresses.
    This allows the attacker to probe which ports are open while also overwhelming the server with SYN requests.

### Analysis:

In this scenario, the attacker not only floods the target with a large number of TCP SYN packets but its coming from different IP addresses and the source ports in an incremental manner.

```
tcpdump -nr threat_actor.pcap 'tcp[13] $ 2!=0' and ! src net 192.168.10.0/24 -c 20
```

![Pasted image 20240327164544](https://github.com/lm3nitro/Projects/assets/55665256/30414810-fef8-43bc-8f34-6f853194ad9d)

The attacker is conducting a port scan on an internal web server, specifically targeting port 80, by employing source IP address spoofing to conceal the origin of the attack. Furthermore, the use of random source IP addresses makes it challenging to trace the attacker's actual location. Interestingly, all the randomly generated IP addresses used in the attack have a Time-To-Live (TTL) value of 64, indicating that the attacker might be operating from within the network. You can see the TTL in the output below.
```
tcpdump -nr threat_actor.pcap 'tcp[13] $ 2!=0' or tcp[13] & 16!=0' -vvvA -c 1
```
![Pasted image 20240327165516](https://github.com/lm3nitro/Projects/assets/55665256/32ba57a4-79de-4b4c-8b3d-09a13ee98f7f)

In summary, the combination of a TCP SYN flood with a port sweep and incoming packets having a TTL value of 64 suggests a sophisticated and potentially internal attack strategy. Understanding these behaviors is crucial for devising effective defense mechanisms and mitigating the impact of such attacks on network infrastructure and services.

### Effects:

+ Resource Exhaustion: The server becomes overwhelmed with half-open connections, leading to slowdowns or even crashes.
+ Network Congestion: The flood of SYN packets can create significant traffic on the network, contributing to overall congestion and potentially affecting other systems.
+ Increased Difficulty in Mitigation: The varied IP addresses make it challenging for administrators to identify and block the source of the attack without also blocking legitimate traffic.

### Defense: 

+ Connection Limiting: Set limits on the number of simultaneous connections that can be established from any single IP address, reducing the effectiveness of distributed SYN flooding.
+ Load Balancing: Implement load balancers to distribute incoming SYN traffic across multiple servers, helping to absorb the impact of SYN floods and maintain service availability.
+ Adjust Timeout Settings: Set shorter timeout values for half-open connections on servers. This will help free up resources more quickly in the event of a SYN flood.

In summary, while the SYN flood attack with port sweep primarily threatens availability, it can also create conditions that indirectly impact confidentiality and integrity.
