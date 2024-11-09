# Cisco NetFlow Configuration

<img width="403" alt="Screenshot 2024-10-05 at 3 25 54 PM" src="https://github.com/user-attachments/assets/f681b7a5-e2e1-4965-802e-7fdb1fcaaf80">

Network management protocols like SNMP enable us to monitor various aspects of our network, such as CPU load, memory usage, interface status, and interface load. Tools like NBAR help identify which protocols are being used. However, these tools can't track all the individual flows within a network. A flow consists of a stream of packets that share common characteristics like source and destination ports, IP addresses, protocol, service type, and more. NetFlow fills this gap by allowing us to monitor these flows across the network. This data is invaluable for diagnosing issues like bottlenecks, identifying which applications are in use, and determining their bandwidth consumption.

NetFlow tracks key metrics for each flow, including the number of packets sent, total bytes transferred, packet sizes, and more. You can configure your router to monitor all flows and export this data to a central server for detailed traffic analysis.

### Scope:

In this lab, I will demonstrate the process of configuring NetFlow v9 on a Cisco IOS router, detailing how to enable and monitor network flows to gain insights into traffic patterns. This will be Part 1 of this lab where I will be configuring the router to collect and export Netlow data. In part 2, I will be setting up a NetFlow collector (SiLK), and analyzing flow metrics to interpret traffic trends, which is critical for network monitoring and security.

### Tools and Technology

Cisco router, Zenmap, and Wireshark

### Network Diagram:

![Pasted image 20240527135759](https://github.com/lm3nitro/Projects/assets/55665256/96eff4b5-f78f-44fb-9da0-9648f9f76d12)

## Getting Started

Below are pictures of the Cisco Router that I have in my home and will be using:

![Pasted image 20240527151636](https://github.com/lm3nitro/Projects/assets/55665256/facfed05-275a-4433-817d-777418301677)

![Pasted image 20240527151752](https://github.com/lm3nitro/Projects/assets/55665256/fcec3de9-d008-469a-af06-3fad2dc7a765)

![Pasted image 20240527151852](https://github.com/lm3nitro/Projects/assets/55665256/82130f2b-2572-4990-9787-425452ef03bd)

First thing is to boot up the router:

![Pasted image 20240527105109](https://github.com/lm3nitro/Projects/assets/55665256/7173568f-5a92-42d8-a8a9-59c0e7ce0ee6)

Information on the model and license:

![Pasted image 20240527105321](https://github.com/lm3nitro/Projects/assets/55665256/1f494a16-2834-443d-83f7-6ca187c8307c)

Below is the configuration I used:

![Pasted image 20240527111454](https://github.com/lm3nitro/Projects/assets/55665256/1ed26cdd-895f-4ddc-a6d5-3289d1ac8b0e)

Configuring DHCP for the LAN_NETWORK:

![Pasted image 20240527123457](https://github.com/lm3nitro/Projects/assets/55665256/1e281444-bd5a-4e38-ad2d-770079e786de)

Client side DHCP traffic:

![Pasted image 20240527112902](https://github.com/lm3nitro/Projects/assets/55665256/75042c71-8b57-4c70-9eaa-ca156fd1f311)

On the client:

![Pasted image 20240527113026](https://github.com/lm3nitro/Projects/assets/55665256/ab4ee537-c7b4-4eee-9822-6cf29fc4f216)

## Displaying a specific DHCP active lease, or all active leases: 

```
show ip dhcp binding
```

![Pasted image 20240528081836](https://github.com/lm3nitro/Projects/assets/55665256/ce97d6d4-f03d-448e-8cbf-3dc2b59df3bf)

## NetFlow configuration:

To configure NetFlow, I first created a NetFlow profile to assign the IP of the NetFlow collector and the source IP of the interface I wanted to collect from as well as the port:

```
flow exporter lm3nitro_netflow
 destination 172.16.10.10
 source GigabitEthernet0/0
 transport udp 9996
```

Then I configured a monitoring interface:

```
flow monitor lm3nitro_monitor
exporter lm3nitro_netflow
record netflow ipv4 original-input
```

This is the current output from the running configuration:

![Pasted image 20240527114210](https://github.com/lm3nitro/Projects/assets/55665256/180b9ced-7c5c-4004-8fc9-fdd5262647ed)

##Verification:

![Pasted image 20240527113517](https://github.com/lm3nitro/Projects/assets/55665256/8cc96c80-5a25-42e4-a2e7-a571a27fa815)

![Pasted image 20240527114022](https://github.com/lm3nitro/Projects/assets/55665256/9dfd4fc2-e466-4a40-8120-67fff92186a8)

I then applied the monitoring NetFlow profile called "lm3nitro_monitor" to an interface:

> [!IMPORTANT]  
> Note: The command needs to be applied on the actual interface.

```
ip flow monitor lm3nitro_monitor input
```

![Pasted image 20240527115028](https://github.com/lm3nitro/Projects/assets/55665256/51c77bc4-8b13-4aab-880c-8fc2a628f601)

Now that I got that configured, I went to see what information about the flow records I had, at the moment I didn't have any cached entries:

```
show flow monitor name lm3nitro_monitor cache
```

![Pasted image 20240527115445](https://github.com/lm3nitro/Projects/assets/55665256/757308da-c6d4-4594-8946-e88f7a489824)

Since there was nothing, I generated some traffic using Zenmap:

> [!NOTE]  
>The router is getting an IP by DHCP from the network 172.16.1.0/24. I'm generating traffic to the Firewall gateway 10.10.100.1 which is located on the "gi0/1 WAN_INTERFACE". Also, Zemmap won't be able to pick up any information from the firewall because it's blocking all the probes:

![Pasted image 20240527120720](https://github.com/lm3nitro/Projects/assets/55665256/1bb007ee-1270-4c1f-b9d1-41d047f78376)

Zemmap:

![Pasted image 20240527120501](https://github.com/lm3nitro/Projects/assets/55665256/769e45d1-db83-410b-9443-227c6b1313cc)

Let's check the NetFlow cache again:

![Pasted image 20240527120624](https://github.com/lm3nitro/Projects/assets/55665256/680eb29d-8e94-41fe-aba6-386b056b7978)

Below is the NetFlow traffic sent to the NetFlow collector by the router:

![Pasted image 20240527121713](https://github.com/lm3nitro/Projects/assets/55665256/45b46952-c21f-4d59-9a0f-5864083b190a)

Details of the flows:

![Pasted image 20240527121941](https://github.com/lm3nitro/Projects/assets/55665256/b466ac64-246b-4348-8b4f-a97f0ed9fc82)

## NAT configuration
I then configured NAT/PAT on the Cisco Router. To do this, I have to select the interfaces that are outside and inside.

```
interface gi1/0
  ip nat outside

interface gi0/0
  ip nat inside
```

![Pasted image 20240527125004](https://github.com/lm3nitro/Projects/assets/55665256/856395a8-d067-4cea-980f-bf15adb65be9)

I also created an access list to allow traffic to the translated:

```
ip nat inside source list 1 interface GigabitEthernet0/1 overload
ip route 0.0.0.0 0.0.0.0 172.16.1.1
```

![Pasted image 20240527124004](https://github.com/lm3nitro/Projects/assets/55665256/85fb07e3-a36e-4f21-a0ce-cd824def2848)

After, I created a static route pointing to Firewall on 172.16.1.1:
 
![Pasted image 20240527124747](https://github.com/lm3nitro/Projects/assets/55665256/99fe00e8-4d25-4f1c-8e95-c8e6ea251282)

Let's look at the NAT translations:

```
sh ip nat translations
```

![Pasted image 20240527124225](https://github.com/lm3nitro/Projects/assets/55665256/f2b76dce-f920-4042-9c7c-e7bbd2ac084f)

Let's look at the NetFlow cache entries again, now we have some entries:

![Pasted image 20240527125355](https://github.com/lm3nitro/Projects/assets/55665256/794f47d6-b83d-4482-bbe0-0e0e21bfbdb4)

Tracking flows on Gi0/0 interface:

> [!NOTE]  
> The IP route-cache flow command will track all ingress flows on the physical interface and all sub-interfaces. You can also use the IP flow egress or IP flow ingress commands if you only want to enable it on one sub-interface or in one direction.

![Pasted image 20240527132357](https://github.com/lm3nitro/Projects/assets/55665256/cd41312d-f79c-46bb-85f4-43c188dba955)

Let's generate more traffic:

![Pasted image 20240527132654](https://github.com/lm3nitro/Projects/assets/55665256/f2683e91-cd15-4b7b-96c9-3f3224c3ff68)

## Tunning "NetFlow":

This is optional configuration:

```
interface Gi0/0
+ ip address 172.16.10.1 255.255.255.0
+ flow-sampler my-map
+ !
+ !
+ flow-sampler-map my-map
+ mode random one-out-of 5
+ !
+ ip flow-cache timeout inactive 60
+ ip flow-cache timeout active 1
+ ip flow-capture fragment-offset
+ ip flow-capture packet-length
+ ip flow-capture ttl
+ ip flow-capture vlan-id
+ ip flow-capture icmp
+ ip flow-capture ip-id
+ ip flow-capture mac-addresses
+ ip flow-export version 9
+ ip flow-export template options export-stats
+ ip flow-export template options sampler
+ ip flow-export template options timeout-rate 1
+ ip flow-export template timeout-rate 1
+ ip flow-export destination 172.16.10.10 9996
```

### Summary:

From this lab, I learned how to effectively configure and utilize NetFlow v9on a Cisco router, gaining practical experience in network monitoring and traffic analysis. This reinforced the importance of understanding network behavior for effective troubleshooting and performance optimization, as well as the value of centralized data collection for informed decision-making.

Having a router configured with NetFlow provides deep visibility into network performance which allows us to track and analyze traffic patterns in real time. This visibility is essential for identifying bandwidth issues, understanding application usage, and detecting anomalies or security threats, which can help mitigate risks before they escalate into significant issues.

This lab is continue in part 2 **Cisco Netflow to SiLK**


