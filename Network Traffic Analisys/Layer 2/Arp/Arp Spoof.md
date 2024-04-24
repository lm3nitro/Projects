ARP Spoofing




A key red flag we need to monitor is any anomaly in traffic emanating from a specific host. For instance, one host incessantly broadcasting ARP requests and replies to another host could be a indicator of ARP spoofing.


![Pasted image 20240404123724](https://github.com/lm3nitro/Projects/assets/55665256/18ac919f-e3b2-43d1-924d-20b370481de9)



 We can analyse both requests and replies among the attacker's machine, the victim's machine, and the router.


The opcode functionality in Wireshark can help with this process.

Opcode == 1: This represents all types of ARP Requests
Opcode == 2: This signifies all types of ARP Replies


![Pasted image 20240404122012](https://github.com/lm3nitro/Projects/assets/55665256/ebb2a3e4-53f8-484d-88cb-02915a8e598e)


we can see that one IP address is mapped to two different MAC addresses. We can validate this on a Linux system by executing the appropriate commands.

lm3nitro@lm3nitro]$ arp -a | grep 50:eb:f6:ec:0e:7f

 (192.168.10.4) at 50:eb:f6:ec:0e:7f [ether] on ens32

lm3nitro@lm3nitro]$ arp -a | grep 08:00:27:53:0c:ba

 (192.168.10.4) at 08:00:27:53:0c:ba [ether] on ens32


 Our ARP cache, in fact, contains both MAC addresses allocated to the same IP address - an anomaly that warrants our immediate attention.


Detecting more duplicate ARP records


Filter:
arp.duplicate-address-detected && arp.opcode == 2

![Pasted image 20240404122945](https://github.com/lm3nitro/Projects/assets/55665256/0ad68d16-3096-4959-bf80-648ae1ab95b0)


# Identifying The Original IP Addresses:

A crucial question we need to pose is, what were the initial IP addresses of these devices? Understanding this aids us in determining which device altered its IP address through MAC spoofing. After all, if this attack was exclusively performed via ARP, the victim machine's IP address should remain consistent. Conversely, the attacker's machine might possess a different historical IP address.

Filter:

(arp.opcode) && ((eth.src == 08:00:27:53:0c:ba) || (eth.dst == 08:00:27:53:0c:ba))

![Pasted image 20240404123425](https://github.com/lm3nitro/Projects/assets/55665256/2c9392f7-f2eb-455c-bf3a-0d13f2c2dcf1)

In this case, we might instantly note that the MAC address 08:00:27:53:0c:ba was initially linked to the IP address 192.168.10.5, but this was recently switched to 192.168.10.4. This transition is indicative of a deliberate attempt at ARP spoofing or cache poisoning.


Examining the traffic from these MAC addresses:

Filter:

eth.addr == 50:eb:f6:ec:0e:7f or eth.addr == 08:00:27:53:0c:ba

![Pasted image 20240404124206](https://github.com/lm3nitro/Projects/assets/55665256/362f3364-89dd-4610-8353-3a7cf2babf69)


Right off the bat, we might notice some inconsistencies with TCP connections. If TCP connections are consistently dropping, it's an indication that the attacker is not forwarding traffic between the victim and the router.

If the attacker is, in fact, forwarding the traffic and is operating as a man-in-the-middle, we might observe identical or nearly symmetrical transmissions from the victim to the attacker and from the attacker to the router.
