# UDP Anomolies

A UDP anomaly refers to any unusual pattern or behavior in UDP traffic that deviates from expected norms. Since UDP is a connectionless protocol, anomalies can indicate potential issues, misconfigurations, or security threats.

### Scope:

In this exercise I will cover two separete scenarios that demonstrate unusual/anomalous UDP traffic. I will cover the traffic analysis behind both of these scenarios in order to demonstrate common traffic patterns that may be indicative of an attack, and will provide possible mitigations that can help to prevent these attacks. 

### Tools and Technology:
Splunk, TCPdump, Linux, and Wireshark

## Scenario 1: UDP traffic on Non-Standard Port

The network monitoring system detected a sudden spike in UDP traffic on port 12345, a port not typically used by any of your applications. The traffic originates from a single IP address and is targeting a specific internal host.

Below is a view of the UDP traffic spikes:

![Pasted image 20240411124409](https://github.com/user-attachments/assets/b0298557-802f-4954-bf61-2dacfe31d1a3)

Suricata flow Traffic in Splunk: 

![Pasted image 20240411105417](https://github.com/user-attachments/assets/e92bac7f-18c6-481a-ae6c-c08e95ba0b9f)

![Pasted image 20241008105558](https://github.com/user-attachments/assets/75aa79fc-e07c-40f5-b499-a73b99230f47)

I then decided to take a live packet capture with tcpdump to analyze the UDP traffic going to port 12345:

```
sudo tcpdump -ni ens32 udp port 12345
```

![Pasted image 20240411121826](https://github.com/user-attachments/assets/1b9ff9d7-fd70-4cd7-8079-fbe7f8f39e83)

Based on the output, its important to note the all the traffic is coming form a single IP, targeting a single host on UDP port 12345. I can also see that the amount of bytes are mostly the same and then suddenly increase. 

Payload analysis with Tcpdump:

In order to take a closer look at those packets with a alrger length, I creating a filter to capture payload over 64 bytes in length:

```
sudo tcpdump -ni ens32 udp port 12345 and greater 64 -XX
```

![Pasted image 20240411121126](https://github.com/user-attachments/assets/8b1edf11-77a1-42c7-8231-4c4397973709)

Based on the output I can see that this is malicious traffic. 

![Pasted image 20241008105430](https://github.com/user-attachments/assets/c8da32f4-c857-48c2-b5c1-95459e972828)

I also took a closer look at the packet lengths statistics with Wireshark:

![Pasted image 20240411121542](https://github.com/user-attachments/assets/525f5700-0156-424f-8e3f-6e378de8add6)

Took a look at conversations statistics: 

![Pasted image 20240411114316](https://github.com/user-attachments/assets/78ba84ae-3127-4808-97b0-6831809506b9)

Also took a look at the protocol hierarchy:

![Pasted image 20240411114359](https://github.com/user-attachments/assets/4ad1fae0-2b08-4253-8d53-9d2a8e3280b8)

I was also able to see the same message seen above using tcpdump in Wireshark. Protocol spoofing and tunneling methods can circumvent network security protocols, avoid detection, and enable unauthorized data exfiltration, command and control (C2) communications, or lateral movement within the network.

![Pasted image 20240411130628](https://github.com/user-attachments/assets/f3758be3-3f03-46ee-9d95-e3fa07e289eb)

### Mitigation:

In order to defend against these types of attacks, you can do the following:
+ Restrict Unused Protocols: Disabling unused ports and services reduces the attack surface.
+ Monitor for Anomalous Behavior: Set up alerts for unusual traffic patterns which could indicate tunneling activities.
+ Firewalls and Access Control Lists: Configure firewalls to block or restrict traffic on non-standard UDP ports. Create ACLs that only allow traffic on known, necessary ports.

## Scenario: UDP Flood DDoS Attack:

In the scenario, I will simulate a UDP flood DDoS attack that will overwhelm the server resources and disrupt essential services. The goal is to observe the traffic behavior and learn how to identify and defend against this type of attack effectively.

After initiating the simulation attack on my target VM, the first inidication was the CPU utilization:

![Pasted image 20241008104703](https://github.com/user-attachments/assets/5b8eda41-f52c-4a27-a67a-00f246447b00)

Also the network card utilization: 

![Pasted image 20241008105009](https://github.com/user-attachments/assets/575fd586-4c87-4579-b910-edb64f2ed889)

### Detecting UDP flood attacks

Common indicators of a UDP flood attack include:
+ Sudden Increase in UDP Traffic
+ Unusual Source IP Addresses
+ High CPU and Memory Usage

In order to analyze the network traffic, I utilized Wireshark. 

![Pasted image 20241008105416](https://github.com/user-attachments/assets/453ec337-3b9d-4d22-8682-7cec51bc35c8)

Protocol hierarchy, a majority of the traffic being UDP:

![Pasted image 20240411161413](https://github.com/user-attachments/assets/c3901459-1674-4c08-a58f-b115ba6cd569)

large amoujnt of conversations all originating from a series of different IP addresses:

![Pasted image 20240411153412](https://github.com/user-attachments/assets/eb61faf0-0267-4e62-925b-be2d12fbde74)

Wireshark expert information: 

![Pasted image 20240411153123](https://github.com/user-attachments/assets/acd56d34-be49-491d-9911-b089b368db06)

Visualizing packet lengths:

![Pasted image 20240411154027](https://github.com/user-attachments/assets/74aac863-fdfb-4c20-aa0c-2b5c62d2c3d1)

Taking a close look at the average amount of the packets sent:

![Pasted image 20240411152810](https://github.com/user-attachments/assets/a83c6897-072e-462c-b86d-3374eae8a7c6)

Payload analysis: 

![Pasted image 20240411153934](https://github.com/user-attachments/assets/5ddc32d1-68a9-4a12-aff3-dd324f3f9aaa)

### Mitigation:

Here’s a list of mitigations to defend against UDP flood attacks:
+ Set thresholds on the amount of UDP traffic allowed per IP address to prevent overwhelming the network.
+ Utilize IDS/IPS to detect and respond to anomalous UDP traffic patterns indicative of a flood attack.
+ Continuously monitor network traffic for unusual spikes or patterns
+ Establish limits on the number of simultaneous connections that can be made to specific services, helping to prevent resource exhaustion.

### Summary:

In these scenarios, I was able to simulate anomalous UDP behavior, specifically focusing on non-standard ports and a UDP flood attack. This hands-on experience allowed me to analyze network traffic during these events and identify common patterns that indicate malicious activity. Through this exercise, I learned to better recognize the characteristics and behaviors associated with these types of attacks, including shifts in traffic volume, variations in packet size, and changes in communication patterns among devices. 
