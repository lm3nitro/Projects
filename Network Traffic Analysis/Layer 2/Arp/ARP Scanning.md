# ARP Scanning

By closely examining ARP requests and replies, we can spot more unusual behaviors. It's widely known that ARP attacks like poisoning and spoofing are often used in DoS and MITM attacks. Yet, attackers might also use ARP to gather information. Fortunately, we have the ability to detect and assess these tactics using familiar methods. Below, I examine several ways we can detect these unusual behaviors.

## Signs of ARP Scanning

In the following examples, I was closely following the traffic involving 192.168.10.5. Here are some typical red flags indicative of ARP scanning are the following. 

1. Broadcast ARP requests sent to sequential IP addresses (.1,.2,.3,...). This is because it suggests a systematic attempt to discover active hosts within a network. 

![Pasted image 20240404141016](https://github.com/lm3nitro/Projects/assets/55665256/ed12c5c4-6cd8-4db4-a633-89387214f4ef)


2. Broadcast ARP requests sent to non-existent hosts. By observing responses or lack thereof to these broadcast ARP requests, attackers can gather information about the network and its hosts.

![Pasted image 20240404141737](https://github.com/lm3nitro/Projects/assets/55665256/fd71f047-15ed-478a-9c90-7498eac6bff5)


3. Here I was able to see the hosts that actually replied to the 192.168.10.5:
   
![Pasted image 20240404141620](https://github.com/lm3nitro/Projects/assets/55665256/d0b7f3ec-fa99-47e4-af50-23e1a39c4824)

5. Another indication can be seeing an unusual volume of ARP traffic originating from a malicious or compromised host. In the table below, we see exactly that:

![Pasted image 20240404140858](https://github.com/lm3nitro/Projects/assets/55665256/c9826d8d-1495-419e-8393-440e3bee689c)

It is possible to detect that indeed ARP requests are being propagated by a single host to all IP addresses in a sequential manner.

![Pasted image 20240404141401](https://github.com/lm3nitro/Projects/assets/55665256/bdea9adc-0f6b-4dc3-ba72-3ae13c223f6e)

An attacker could use ARP scanning to create a list of active hosts, then modify their approach to disrupt these machines' services. They might try to infect an entire subnet, manipulating many ARP caches in the process, which is a common tactic for a man-in-the-middle attack. Additionally, the attacker's ARP traffic might start redirecting IP addresses to new MAC addresses, aiming to tamper with the router's ARP cache. Alternatively, if we notice devices receiving duplicate IP addresses like 192.168.10.1, it suggests the attacker is trying to corrupt these devices' ARP caches to block traffic flow bidirectionally.

![Pasted image 20240404142754](https://github.com/lm3nitro/Projects/assets/55665256/563fc144-bd45-4a92-a412-e6f8572dd410)

To prevent ARP scanning in a network, I recommend using ARP spoofing detection tools like intrusion detection systems (IDS) or intrusion prevention systems (IPS). Configuring port security on switches and implementing VLAN segmentation can also assist in preventing ARP scans. It is also important to conduct regular security audits to stay vigilant against potential ARP scanning and spoofing attacks.
