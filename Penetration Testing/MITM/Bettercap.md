# Bettercap

![Pasted image 20240517142357](https://github.com/lm3nitro/Projects/assets/55665256/8a88a4bd-94aa-4b31-8bc5-a702b1bef769)

Bettercap is a powerful, open-source network security tool designed for performing various types of network attacks, monitoring, and traffic manipulation. It is commonly used by security professionals for network reconnaissance, Man-in-the-Middle (MITM) attacks, packet sniffing, and injecting malicious payloads into web traffic. 

### Scope:

In this scenario, the attacker and the target host are connected to the same network via a switch, which is also connected to a firewall. The switch has port mirroring enabled to monitor all network traffic. The attacker will use Bettercap to perform both an ARP spoofing and DNS spoofing attack, poisoning the ARP cache of both the target host and the gateway (firewall), tricking them into routing traffic through the attacker’s machine. With the attacker positioned as a "man-in-the-middle," the attacker will initiate the DNS spoofing attack, intercepting DNS requests from the target and redirecting them to malicious or fake websites. As the attack unfolds, the switch’s port mirroring will be connected to a PC running Wireshark capturing all network traffic for analysis, providing detailed insights into the ARP cache manipulation and DNS spoofing. This behavior will also be captured and monitored in the pfSense firewall.

### Tools and Technology:
Bettercap, Wireshark, Wireshark, Win10, Apache 2, and pfSense

Network Topology:

![Pasted image 20240517211750](https://github.com/lm3nitro/Projects/assets/55665256/51f63d6e-5599-48bf-a67a-5b8bc239fbbb)

## Installing bettercap

Searching for bettercap:

```
apt search bettercap
```

![Pasted image 20240517142617](https://github.com/lm3nitro/Projects/assets/55665256/0ddcc118-2dfe-4e2e-99b5-b64bef0c2077)

Installing bettercap:

```
apt install bettercap
```

![Pasted image 20240517142653](https://github.com/lm3nitro/Projects/assets/55665256/44207384-19c5-49cd-b75a-928f3b3c5b9d)

Getting help with bettercap:

```
bettercap -h
```

![Pasted image 20240517142727](https://github.com/lm3nitro/Projects/assets/55665256/6361046b-87f3-4c3f-8132-d9fa5646251d)

Once installed, I started bettercap:

```
bettercap 
```

![Pasted image 20240517150334](https://github.com/lm3nitro/Projects/assets/55665256/441179a6-7878-4923-b27b-db0454ec02b7)

Next, I needed to enable probing of the network:

```
net.probe on
```

![Pasted image 20240517150556](https://github.com/lm3nitro/Projects/assets/55665256/0ef68117-6424-4920-b730-a57339329116)

I used the following command to list the gateway and any hosts on the network:

```
net.show
```

![Pasted image 20240517150641](https://github.com/lm3nitro/Projects/assets/55665256/d99211bd-bc19-417d-8778-45e448642d8a)

After locating the target host `LM3NITRO-PC`, I enabled ARP spoofing. ARP (Address Resolution Protocol) is a network protocol used to map or translate an IP address (Layer 3) to a MAC address (Layer 2) within a local network.

![Pasted image 20240517151238](https://github.com/lm3nitro/Projects/assets/55665256/8335f958-52e2-4602-9e42-2678e36b51c2)

On the target host itself, I had Wireshark running at the windows PC "Lm3nitro-PC". In the screenshot below, I was able to identify the spoof attack. At the very top I can see that originally, the attacker was assigned the IP 192.168.1.103 had the MAC address ending in 05:7d and the IP address for the pfSense firewall was 192.168.1.1 with a MAC address of 0e:17. After the attack is performed, I see that the attacker is now stating that the pfSense IP address is now its own so that traffic is routed to him rather than to the firewall. 

![Pasted image 20240517153137](https://github.com/lm3nitro/Projects/assets/55665256/cfc0e03a-d977-41d0-be4c-0e350673104e)

Taking a closer look at the packet when the change occurred:

![Pasted image 20240517153359](https://github.com/lm3nitro/Projects/assets/55665256/4566ad42-6498-4074-8fea-8c32f838b886)

Looking at the arp cache of the pc, I was able to see both IP addresses and the same MAC address for each entry:

![Pasted image 20240517151220](https://github.com/lm3nitro/Projects/assets/55665256/d0886a11-098e-4890-b706-a01a593becfd)

## Detecting ARP Spoof Attack:

In order to detect the ARP Spoof attack on the pfSense firewall, I installed Arpwatch. To do this, I went to `System > Package Manager`

![Pasted image 20240517161852](https://github.com/lm3nitro/Projects/assets/55665256/abf38603-7b63-4844-91a7-7adaed9b0e7c)

Installed it:

![Pasted image 20240517162104](https://github.com/lm3nitro/Projects/assets/55665256/4a1e2daa-b0e9-4c8e-ae04-848afcc1b1c1)

![Pasted image 20240517162135](https://github.com/lm3nitro/Projects/assets/55665256/a697389b-ca83-4ffc-b9ec-2235ecdc3b4d)

Once installed, navigated to  `Services > Arpwatch`:

![Pasted image 20240517162226](https://github.com/lm3nitro/Projects/assets/55665256/f04eae82-633f-40f3-a61c-6cf91a208879)

Configured it:

![Pasted image 20240517162409](https://github.com/lm3nitro/Projects/assets/55665256/71791e98-3c8b-495f-9079-ba2bf3198aaf)

This is what the normal ARP table looks like:

![Pasted image 20240517162815](https://github.com/lm3nitro/Projects/assets/55665256/447d185a-000a-4d04-9fe9-c7e223feeca7)

After performing the ARP spoof, I can see that the table now shows duplicate addresses for the 192.168.1.1 IP:

![Pasted image 20240517162843](https://github.com/lm3nitro/Projects/assets/55665256/6898b8c6-bd43-4d3a-a024-6e500ed74f50)

On the network:

![Pasted image 20240517164505](https://github.com/lm3nitro/Projects/assets/55665256/7c0baf7f-f1f6-41b0-af68-92fe0f7e33d8)

I was also able to see a lot of ARP traffic/broadcast traffic from a single IP:
 
![Pasted image 20240517164714](https://github.com/lm3nitro/Projects/assets/55665256/4039cdf0-8d98-47f2-a57d-0b3d949ffcff)

## Enabling sniffing:

After verifying the above, I enabled sniffing on the attacking host using bettercap:

```
net.sniff on
```

Enabling sniffing allowed me to see the target host `lm3nitro-PC` visiting chess.com. The traffic had been sniffed by the attacker pc on the bottom:

![Pasted image 20240517155121](https://github.com/lm3nitro/Projects/assets/55665256/6d844ee1-6b37-4130-859f-e4e5854af2ab)

Targer host (lm3nitro-PC):

![Pasted image 20240517154646](https://github.com/lm3nitro/Projects/assets/55665256/893c4f6e-7293-4c0e-b8ef-faec5eefd9d4)

## DNS Spoofing Attack:

In order to perform the DNS spoof attack, I needed a webserver. I used Apache 2:

![Pasted image 20240517155521](https://github.com/lm3nitro/Projects/assets/55665256/f28423bf-ce28-4477-98c6-8d2c51c419c0)

Using bettercap, I set up the DNS spoof:

```
set. dns.spoof.domains chess.com
dns.spoof on
```

![Pasted image 20240517155654](https://github.com/lm3nitro/Projects/assets/55665256/3a0b38a0-9814-4660-88ae-192c2cdbb5cb)

On the target host (lm3nitro-PC) I navigated again to chess.com. I was routed to the Apache2 webserver:

![Pasted image 20240517160544](https://github.com/lm3nitro/Projects/assets/55665256/c27d07f4-4f1f-44e1-858b-1d853c5bf823)

Output from the Attacker PC:

![Pasted image 20240517160818](https://github.com/lm3nitro/Projects/assets/55665256/b0c70da2-b6ef-4a3f-8c12-59a9e753f62e)

### Summary

In this exercise, I was able to emulate an ARP spoofing and DNS Spoofing attack using Bettercap. This allowed me to understand how attackers can manipulate network traffic and redirect victims to malicious sites by poisoning ARP tables and intercepting DNS requests. By carrying out these attacks, I was able to manipulate how the target device identified the network gateway, effectively diverting traffic through the attacker's machine. Additionally, I learned to recognize the signs of an attack, such as unusual network behavior and changes in ARP cache entries (arp -a showing unexpected MAC addresses). This allowed me to gain insight into how these attacks work, the signs of an attack, and how to defend against them. The following measures can be taken to defend against these types of attacks:

+ Use Static ARP Entries
+ Implement Network Segmentation
+ Configure DNS Security
