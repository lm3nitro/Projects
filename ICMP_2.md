

![Pasted image 20240325233711](https://github.com/lm3nitro/Projects/assets/55665256/f74024b1-c25b-4779-b500-6b5f929b5332)


Step 1:
Note: In this pcap we have 8330 packets we will need to filter to find out the abnormal packets. 
 
 Filter for ICMP number of  packets :



Identifying  IP Source & Destination Spoofing Attacks:

Let's consider the following when analyzing these fields for our traffic analysis efforts.

The Source IP Address should always be from our subnet - If we notice that an incoming packet has an IP source from outside of our local area network, this can be an indicator of packet crafting.

The Source IP for outgoing traffic should always be from our subnet - If the source IP is from a different IP range than our own local area network, this can be an indicator of malicious traffic that is originating from inside our network.

An attacker might conduct these packet crafting attacks towards the source and destination IP addresses for many different reasons or desired outcomes. Here are a few that we can look for:

Decoy Scanning:
Random Source Attack DDoS 
SMURF Attacks
Duplicate Fragments



There are 15 different types of ICMP messages and some are categorized as error reporting and query.

A few popular ICMP messages:

Error Reporting Query
Type Message Type Message
3 Destination Unreachable 8/0 Echo(request/reply)
4 Source quench 13/14 Timestamp(req/Reply)
11 Time exceeded 18/18 Address mask(req/rep)
12 Parameter problem 10-Sep Router advertisement
5 Redirection

#####Scenario we notice high amount of ICMP traffic from our network going to ****


Here is another example where the host from network 192.168.10.5 is responding to random source addresses but this time the payload is larger  it seems strange because ICMP it is usually 64 bytes payload. 

We should also consider that attackers might fragment these random hosts communications in order to draw out more resource exhaustion.

![Pasted image 20240325215127](https://github.com/lm3nitro/Projects/assets/55665256/2a1a793e-7a85-4578-b9b4-e0d8ca9c7dd1)


Lets verify the traffic little be deeper to what we can see.

![Pasted image 20240325224130](https://github.com/lm3nitro/Projects/assets/55665256/b6036d71-520f-4cc4-a977-716d985c61cb)


The threat actor is abusing the protocol by fragmenting ICMP to send a large volume of data to this node, it could to overwhelm the server with bous traffic.  We can see also that the TTL seems to be coming from a internal node. 


Counting the amount of framagment  going to and from 192.168.10.5 ICMP 

![Pasted image 20240325230140](https://github.com/lm3nitro/Projects/assets/55665256/2a53c996-5fa4-448f-b89b-c21b2cb9e8a5)


Let's select one conversation to analyse it to see what is really going on. 

![Pasted image 20240325224948](https://github.com/lm3nitro/Projects/assets/55665256/21865b21-ddea-4b62-8927-f3d32e126100)


The node ended is being hamer with a large amount of fragmented ICMP random spoof request from an internal node.  This can cause  a denial of services (DoS) on node 192.168.10.5. 

Identify Smurf Attacks:

![Pasted image 20240325233000](https://github.com/lm3nitro/Projects/assets/55665256/57e3dd74-2036-4885-a751-6671d0e5a669)

One of the things we can look for in our traffic behavior is an excessive amount of ICMP replies from a single host to our affected host. Sometimes attackers will include fragmentation and data on these ICMP requests to make the traffic volume larger.

![Pasted image 20240325233127](https://github.com/lm3nitro/Projects/assets/55665256/a9623975-6891-495c-80e2-86931112cc38)

SMURF Attacks are a notable distributed denial-of-service attack, in the nature that they operate through causing random hosts to ping the victim host back. Simply put, an attacker conducts these like the following:

1- The attacker will send an ICMP request to live hosts with a spoofed address of the victim host

2- The live hosts will respond to the legitimate victim host with an ICMP reply

3- This may cause resource exhaustion on the victim host



################
Identify overlapping / duplicate fragments
**Teardrop**: 



![Pasted image 20240326134730](https://github.com/lm3nitro/Projects/assets/55665256/6744eff6-3ab6-4a42-bc39-259cd6a5a86b)


The first indicator of this behaviour is the fragmentation of the data on the ICMP that are split in 3 chunks of 40 bytes each, also the overlaying fragment on the third packet, that could indicate some on the network is train to bypass an IDS. One way to identify  overlapping fragments is to see the last ICMP fragment packet that has offset 80, flags [none] This mean that the host 192.168.11.13 has sent 3 fragmented packets  of 40 bytes and host 192.168.11.61 only received 2 with has 80 bytes total. 

A Fragment Overlap Attack, also known as an IP Fragmentation Attack, is an attack that is based on how the Internet Protocol (IP) requires data to be transmitted and processed. These attacks are a form of Denial of Service (DoS) attack where the attacker overloads a network by exploiting datagram fragmentation mechanisms. 


Identify  suspicions host using traceroute on the network:


![Pasted image 20240326151132](https://github.com/lm3nitro/Projects/assets/55665256/2f5bb032-63fb-41d4-b800-38a5f9e4ec0c)


![Pasted image 20240326151607](https://github.com/lm3nitro/Projects/assets/55665256/6ec1ea9b-efd1-414e-b241-52ed9bb85332)



Threat actor usually try to map the network by using tools like traceroute.  Something to look for is the low TTL from  the source, low packet size and the high amount the ICMP use.  The victim host that is being is sending it's real TTL.

Identify: 
ICMP  spoof  same source and destination with incremental fragmentation

![Pasted image 20240326155509](https://github.com/lm3nitro/Projects/assets/55665256/b1912aaf-1906-4f18-8ab0-4d4a6ec36e75)

