# ARP Scanning

ARP scanning is a method used to map IP addresses to MAC addresses on a local network by sending ARP requests. Devices respond with their MAC address, allowing the scanner to create a list of active network devices. It is commonly used for network inventory, security monitoring, and troubleshooting.

### Scope:

I will reviewing a pcap related to ARP traffic and will go over several signs that can indicate ARP scanning activity within the network. It's widely known that ARP attacks like ARP poisoning and spoofing are often used in DoS and MITM attacks. Yet, attackers might also use ARP to gather information. Fortunately, we have the ability to detect and assess these tactics using familiar methods. 

## ARP Traffic Analysis

In the example below, I see broadcast ARP requests sent to sequential IP addresses (.1,.2,.3,...). This suggests a systematic attempt to discover active hosts within a network:

![Pasted image 20240404141016](https://github.com/lm3nitro/Projects/assets/55665256/ed12c5c4-6cd8-4db4-a633-89387214f4ef)

By observing responses or lack thereof to these broadcast ARP requests, attackers can gather information about the network and its hosts.

![Pasted image 20240404141737](https://github.com/lm3nitro/Projects/assets/55665256/fd71f047-15ed-478a-9c90-7498eac6bff5)

These are the hosts that actually replied to the host 192.168.10.5:
   
![Pasted image 20240404141620](https://github.com/lm3nitro/Projects/assets/55665256/d0b7f3ec-fa99-47e4-af50-23e1a39c4824)

Another indication is seeing an unusual volume of ARP traffic originating from a malicious or compromised host. In the table below, this is seen:

![Pasted image 20240404140858](https://github.com/lm3nitro/Projects/assets/55665256/c9826d8d-1495-419e-8393-440e3bee689c)

Below is another view of the abnormal amount of traffic related to ARP:

![Pasted image 20240404141401](https://github.com/lm3nitro/Projects/assets/55665256/bdea9adc-0f6b-4dc3-ba72-3ae13c223f6e)

An attacker could use ARP scanning to create a list of active hosts, then modify their approach to disrupt these machines' services. They might try to infect an entire subnet, manipulating many ARP caches in the process, which is a common tactic for a man-in-the-middle attack. Additionally, the attacker's ARP traffic might start redirecting IP addresses to new MAC addresses, aiming to tamper with the router's ARP cache. Duplicate addresses as seen in the scrrnshot below if another indication; it suggests the attacker is trying to corrupt these devices' ARP caches to block traffic flow bi-directionally.

![Pasted image 20240404142754](https://github.com/lm3nitro/Projects/assets/55665256/563fc144-bd45-4a92-a412-e6f8572dd410)

### Summary

It is imporant to continually monitor traffic and look out for signs that may indicate malicious activity. By continuously monitoring it's possible identify potential reconnaissance activity by attackers or unauthorized devices attempting to map the network. 

Mitigation:

Here are three effective mitigations to prevent or detect ARP scanning in a network:

+ Configure IP-MAC binding on devices or network switches to ensure only known IP-to-MAC address pairs are valid. This creates a trusted map of devices and prevents attackers from impersonating legitimate devices via ARP poisoning.

+ Use tools or network monitoring software that can detect unusual ARP activity, such as multiple devices claiming the same IP address or sudden changes in ARP tables, and alert in case ARP scanning or spoofing attempt is detected.

+ Segment the network into smaller subnets or VLANs to limit the scope of ARP traffic. This reduces the impact of ARP scanning, as it confines the discovery process to smaller network areas, making it harder for attackers to gather information across the entire network.
