# ARP Spoofing

ARP spoofing (also known as ARP poisoning) is a type of cyberattack in which an attacker sends false ARP messages onto a local network. These malicious ARP messages associate the attacker's MAC address with the IP address of a legitimate device on the network. This causes traffic intended for that device to be redirected to the attacker instead.

### Scope:

In this scenario, you are monitoring network traffic for potential security issues. One morning, you receive an alert that indicates ARP spoofing activity on the network. This alert includes a PCAP file containing network traffic captured at the time of the suspicious activity. Your task is to analyze the PCAP file to verify the ARP spoofing attack. 

## Analysis

I have taken the provided pcap file and and review the traffic using Wireshark. Taking a look at the traffic in Wireshark, I first went to the graphs and filtered for the ARP traffic and could see the abnormal amount of traffic:

![Pasted image 20240404123724](https://github.com/lm3nitro/Projects/assets/55665256/18ac919f-e3b2-43d1-924d-20b370481de9)

Next, I want to take a closer look at both the requests and replies. The opcode functionality in Wireshark can help with this process.

+ Opcode == 1: This represents all types of ARP Requests
+ Opcode == 2: This signifies all types of ARP Replies

![Pasted image 20240404122012](https://github.com/lm3nitro/Projects/assets/55665256/ebb2a3e4-53f8-484d-88cb-02915a8e598e)

When filtering for the ARP requests, I see that there is one particular IP address that is mapped to two different MAC addresses.To further validate this behavior, I went to the host on 192.168.10.4 and ran the following commands:

```
lm3nitro@lm3nitro]$ arp -a | grep 50:eb:f6:ec:0e:7f
```

Output:(192.168.10.4) at 50:eb:f6:ec:0e:7f [ether] on ens32

```
lm3nitro@lm3nitro]$ arp -a | grep 08:00:27:53:0c:ba
```

Output: (192.168.10.4) at 08:00:27:53:0c:ba [ether] on ens32

Based on the output above, I can see that the ARP cache, in fact, contains both MAC addresses and that they are both allocated to the same IP address - an anomaly that warrants immediate attention.

Going back to Wireshark, I used the following filter to view ARP replies that also contain a duplicate address message:

```
arp.duplicate-address-detected && arp.opcode == 2
```

![Pasted image 20240404122945](https://github.com/lm3nitro/Projects/assets/55665256/0ad68d16-3096-4959-bf80-648ae1ab95b0)

> [!NOTE]  
> When analyzing ARP spoofing, an important question to consider is: What were the original IP addresses of these devices? This information is key to identifying which device may have altered its IP address through MAC spoofing. If the attack was solely ARP-based, the victim's IP address should remain unchanged. However, the attacker’s device might have previously used a different IP address.

Next, I used the following filter:

```
(arp.opcode) && ((eth.src == 08:00:27:53:0c:ba) || (eth.dst == 08:00:27:53:0c:ba))
```

![Pasted image 20240404123425](https://github.com/lm3nitro/Projects/assets/55665256/2c9392f7-f2eb-455c-bf3a-0d13f2c2dcf1)

Here I can see that the MAC address 08:00:27:53:0c:ba was initially linked to the IP address 192.168.10.5, but was then switched to 192.168.10.4. This transition is indicative of a deliberate attempt at ARP spoofing or cache poisoning.

To further examine the traffic from these MAC addresses, I used the following filter:

```
eth.addr == 50:eb:f6:ec:0e:7f or eth.addr == 08:00:27:53:0c:ba
```

![Pasted image 20240404124206](https://github.com/lm3nitro/Projects/assets/55665256/362f3364-89dd-4610-8353-3a7cf2babf69)

This filter allowed me to see some inconsistencies with the TCP connections. If TCP connections are consistently dropping, it's an indication that the attacker is not forwarding traffic between the victim and the router. If the attacker is, in fact, forwarding the traffic and is operating as a man-in-the-middle, we might observe identical or nearly symmetrical transmissions from the victim to the attacker and from the attacker to the router.

### Summary

Upon analysis, the behavior analyzed suggests that ARP spoofing has occurred and immediate remediation steps must be taken to stop the ongoing attack and mitigate any damage. This may include disconnecting the affected devices from the network, especially the attacker’s machine, to stop any further malicious ARP poisoning. Also, clearing the ARP cache is necessary in order to remove any incorrect IP-to-MAC mappings caused by the spoofing attack.

Additionally, proactive measures should be implemented to prevent future ARP spoofing attacks. Here are some preventative measures that can be used:

+ Enable Dynamic ARP Inspection (DAI) on Switches: This helps prevent ARP spoofing by ensuring that only valid ARP requests and replies are allowed based on the trusted ARP entries or DHCP snooping database
+ Implement Static ARP Entries for Critical Devices: For network devices like routers, gateways, and servers, configure static ARP entries. This ensures that their IP-to-MAC mappings are fixed and cannot be overwritten by malicious ARP replies.
+ Configure Port Security on Network Switches: Port security allows you to limit the number of MAC addresses that can be associated with a single port on a switch, helping prevent ARP spoofing attacks by limiting access to legitimate devices.
+ Regularly Monitor and Review ARP Traffic: Set up alerts for any signs of ARP anomalies or ARP poisoning, so that suspicious activities can be detected in real-time. 
