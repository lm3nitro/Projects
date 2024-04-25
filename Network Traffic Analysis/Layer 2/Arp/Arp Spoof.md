# ARP Spoofing

I find that it is important to closely monitor network traffic. It is especially important to look into any anomaly in traffic coming from a specific host. An example of this is if one host keeps sending ARP requests and replies nonstop to another host, this could indicate ARP spoofing.

The following graph was taken from Wireshark. Here we see an abnormal amount of arp traffic:

![Pasted image 20240404123724](https://github.com/lm3nitro/Projects/assets/55665256/18ac919f-e3b2-43d1-924d-20b370481de9)

We can analyse both requests and replies among the attacker's machine, the victim's machine, and the router. The opcode functionality in Wireshark can help with this process.

Opcode == 1: This represents all types of ARP Requests
Opcode == 2: This signifies all types of ARP Replies

![Pasted image 20240404122012](https://github.com/lm3nitro/Projects/assets/55665256/ebb2a3e4-53f8-484d-88cb-02915a8e598e)

Here we can see that one particular IP address is mapped to two different MAC addresses. We can further validate this on a Linux system by executing the appropriate commands.
```
lm3nitro@lm3nitro]$ arp -a | grep 50:eb:f6:ec:0e:7f
```
 (192.168.10.4) at 50:eb:f6:ec:0e:7f [ether] on ens32
```
lm3nitro@lm3nitro]$ arp -a | grep 08:00:27:53:0c:ba
```
 (192.168.10.4) at 08:00:27:53:0c:ba [ether] on ens32

Upon investigation, I can see that the ARP cache, in fact, contains both MAC addresses allocated to the same IP address - an anomaly that warrants immediate attention.

Here I can also see more duplicate ARP records. In wireshark I am using the following filter:

arp.duplicate-address-detected && arp.opcode == 2

![Pasted image 20240404122945](https://github.com/lm3nitro/Projects/assets/55665256/0ad68d16-3096-4959-bf80-648ae1ab95b0)

# Identifying The Original IP Addresses:

When it comes to ARP spoofing, a crucial question we need to ask is: What were the initial IP addresses of these devices? Knowing this helps us figure out which device changed its IP address using MAC spoofing. If the attack only involved ARP, the victim's IP address should stay the same. However, the attacker's machine might have had a different IP address in the past.

Here I se the following filter:

(arp.opcode) && ((eth.src == 08:00:27:53:0c:ba) || (eth.dst == 08:00:27:53:0c:ba))

![Pasted image 20240404123425](https://github.com/lm3nitro/Projects/assets/55665256/2c9392f7-f2eb-455c-bf3a-0d13f2c2dcf1)

In this case, we might instantly note that the MAC address 08:00:27:53:0c:ba was initially linked to the IP address 192.168.10.5, but this was recently switched to 192.168.10.4. This transition is indicative of a deliberate attempt at ARP spoofing or cache poisoning.

Now I want to further examine the traffic from these MAC addresses. I use the following filter:

eth.addr == 50:eb:f6:ec:0e:7f or eth.addr == 08:00:27:53:0c:ba

![Pasted image 20240404124206](https://github.com/lm3nitro/Projects/assets/55665256/362f3364-89dd-4610-8353-3a7cf2babf69)


Right off the bat, we might notice some inconsistencies with the TCP connections. If TCP connections are consistently dropping, it's an indication that the attacker is not forwarding traffic between the victim and the router. If the attacker is, in fact, forwarding the traffic and is operating as a man-in-the-middle, we might observe identical or nearly symmetrical transmissions from the victim to the attacker and from the attacker to the router.

