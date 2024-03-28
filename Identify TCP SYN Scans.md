

Filtering for tcp syn flags and port range to identify transport protocol abnormal behaviour

![Pasted image 20240325120655](https://github.com/lm3nitro/Projects/assets/55665256/5faa423d-3c40-4433-b7e0-9b5b151f057c)


Note: It seems like the threat actor on 192.168.11.62 is scanning the node 192.168.11.46 looking for open ports.  We can see the amount sync request going to 192.168.11.46 in many ports in fraction of a second.

Narrow down the host to identify how many synchronization request was send by the threat actor:

![Pasted image 20240325122547](https://github.com/lm3nitro/Projects/assets/55665256/0454d367-56d4-49c1-bd12-5f51240e92fa)


Identify TCP ACK Scans:

![Pasted image 20240327153755](https://github.com/lm3nitro/Projects/assets/55665256/443bb5d8-effc-44b1-9ced-dbbe2fc9ab57)


Identify TCP Fin Scans:

![Pasted image 20240327154309](https://github.com/lm3nitro/Projects/assets/55665256/1895a7b0-0a00-432a-badb-3eae4205eecf)


Identify tcp Xmas scans: 

![Pasted image 20240327155225](https://github.com/lm3nitro/Projects/assets/55665256/a6434005-6b76-438c-b3a1-a6c7435cf034)


Identify tcp Null Scans:

![Pasted image 20240327155836](https://github.com/lm3nitro/Projects/assets/55665256/87b22a4a-5311-48c2-b4ce-18f3005eba0c)


Identify TCP Fragmentation scans:

![Pasted image 20240327160923](https://github.com/lm3nitro/Projects/assets/55665256/289bd943-eaba-4d52-8982-b7a1c1e31ffa)


![Pasted image 20240327161124](https://github.com/lm3nitro/Projects/assets/55665256/82fa873b-031b-4f54-bfca-64be7e1a99fa)

When we begin to look for network anomalies, we should always consider the IP layer. Simply put, the IP layer functions in its ability to transfer packets from one hop to another. This layer uses source and destination IP addresses for inter-host communications. When we examine this traffic, we can identify the IP addresses as they exist within the IP header of the packet.

However, it is essential to note that this layer has no mechanisms to identify when packets are lost, dropped, or otherwise tampered with. Instead, we need to recognize that these mishaps are handled by the transport or application layers for this data. To dissect these packets, we can explore some of their fields:


Length - IP header length: This field contains the overall length of the IP header.
Total Length - IP Datagram/Packet Length: This field specifies the entire length of the IP packet, including 

Fragment Offset: In many cases when a packet is large enough to be divided, the fragmentation offset will be set to provide instructions to reassemble the packet upon delivery to the destination host. 

Source and Destination IP Addresses: These fields contain the origination (source) and destination IP addresses for the two communicating hosts. 


Identify TCP Reset SCANs

![Pasted image 20240327161657](https://github.com/lm3nitro/Projects/assets/55665256/404ceed5-b5a4-4de2-8c07-833817cddf31)

Suppose an adversary wanted to cause denial-of-service conditions within our network. They might employ a simple TCP RST Packet injection attack, or TCP connection termination in simple terms.

This attack is a combination of a few conditions:

1-The attacker will spoof the source address to be the affected machine's
2-The attacker will modify the TCP packet to contain the RST flag to terminate the connection
3-The attacker will specify the destination port to be the same as one currently in use by one of our machines. 

One way we can verify that this is indeed a TCP RST attack is through the physical address of the transmitter of these TCP RST packets. Suppose, the IP address 192.168.10.4 is registered to aa:aa:aa:aa:aa: in our network device list, and we notice an entirely different MAC sending these resets. 

This would indicate malicious activity within our network, and we could conclude that this is likely a TCP RST Attack. However, it is worth noting that an attacker might spoof their MAC address in order to further evade detection. In this case, we could notice retransmissions and other issues as we saw in the ARP poisoning section.


Identify LAN-DoS attacks:
![Pasted image 20240327163548](https://github.com/lm3nitro/Projects/assets/55665256/7bbb6ac9-69b5-4084-b351-0b4c04b54a0d)



Identify TCP SYN SCAN with a  Random spoof source:

![Pasted image 20240327164544](https://github.com/lm3nitro/Projects/assets/55665256/30414810-fef8-43bc-8f34-6f853194ad9d)




The attacker is scanning an internal  web sever on port 80 by spoofing the source with ramdom IPs  to hide the source of the attack, Also it seens like the attacker inside the network since all the ramdom IPs have a TTL of 64

![Pasted image 20240327165516](https://github.com/lm3nitro/Projects/assets/55665256/32ba57a4-79de-4b4c-8b3d-09a13ee98f7f)


Identity TCP hijacking:

![Pasted image 20240327175000](https://github.com/lm3nitro/Projects/assets/55665256/e783d2df-687b-4e13-bab9-23239141d63e)


The attacker will need to block ACKs from reaching the affected machine in order to continue the hijacking. They do this either through delaying or blocking the ACK packets. As such, this attack is very commonly employed with ARP poisoning, and we might notice the following in our traffic analysis.

Identify a large number of ARP requests coming from one node by monitoring ARP protocol.

![Pasted image 20240327200915](https://github.com/lm3nitro/Projects/assets/55665256/de3f64f9-2f34-4363-8d51-ca27913bd238)


In this screenshot show all the ARP request send by  08:00:27:53:0c:ba to  ff:ff:ff:ff:ff:ff :

![Pasted image 20240327202326](https://github.com/lm3nitro/Projects/assets/55665256/b380cb11-5a73-4c7e-b21a-a2ab71516b6b)


The node is flooding the network with ARP request asking where every IP node is at :


![Pasted image 20240327202448](https://github.com/lm3nitro/Projects/assets/55665256/657d58ba-7fc6-4b7d-85eb-3dcd3d08c4c3)


 In this case we see that 192.168.10.5 is sending a large amount of request to all the nodes on the network with the MAC ending in 0c:ba , later the same node impersonate the node with the IP 192.168.10.1 with the MAC ending in b9:4f. At this point the attacker can impersonate any node on the network or create MIT attack to make every request to go trough the attacker.


![Pasted image 20240327181139](https://github.com/lm3nitro/Projects/assets/55665256/59bdf099-e010-4eb0-a54c-a5ec4bbff16d)


By actively monitor the target connection they want to hijack the attacker will then conduct sequence number prediction in order to inject their malicious packets in the correct order. During this injection they will spoof the source address to be the same as our affected machine.  

![Pasted image 20240327174332](https://github.com/lm3nitro/Projects/assets/55665256/43ccec86-983b-4465-aa93-2b2263860f25)


To detect TCP session hijacking using TCP header flags, it is necessary to monitor and analyse the TCP traffic between the hosts for any anomalies or discrepancies in the TCP header flags. An example of this would be unexpected or repeated SYN, ACK, FIN, or RST flags, which can indicate an attempt to initiate, acknowledge, end, or reset a TCP connection without following the normal protocol. Additionally, mismatched or out-of-order sequence numbers may suggest an attempt to inject or modify the TCP packets. Inconsistent or missing PSH or URG flags may also point to an attempt to alter the data or priority of the TCP packets. By monitoring for these signs of TCP session hijacking using TCP header flags, it is possible to detect malicious activity and protect against potential attacks.

Protect against TCP/IP hijacking:

To protect yourself against TCP/IP hijacking, you can take several measures. First, use a reliable antivirus software that can detect and prevent packet sniffing and injection. Second, use encryption technologies such as VPNs or SSL/TLS to encrypt your network traffic. Finally, practice good security hygiene, such as regularly updating your software and using strong passwords.


