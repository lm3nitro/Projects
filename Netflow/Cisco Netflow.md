
# Cisco Netflow Configuration


Network management protocols like SNMP allow us to monitor our network. We can check things like cpu load, memory usage, interface status and even the load of an interface. Other tools like NBAR allow us to see what kind of protocols are used.


![Pasted image 20240527151636](https://github.com/lm3nitro/Projects/assets/55665256/facfed05-275a-4433-817d-777418301677)


One of the things we can’t do with those tools is tracking all flows in our network. A flow is a stream of packets that share the same characteristics like source/destination port, source/destination address, protocol, type, service marking, etc.


![Pasted image 20240527151752](https://github.com/lm3nitro/Projects/assets/55665256/fcec3de9-d008-469a-af06-3fad2dc7a765)


NetFlow allows us to track these flows on our network. We can use this information to solve problems like bottlenecks, identify what applications are used, how much bandwidth they use etc.

For each of the flows, NetFlow will track the number of packets sent, bytes sent, packet sizes and more. You can configure your router to keep track of all flows and then export them to a central server where we analyze our traffic.

![Pasted image 20240527151852](https://github.com/lm3nitro/Projects/assets/55665256/82130f2b-2572-4990-9787-425452ef03bd)


In this labI will show you how to configure NetFlow on a Cisco IOS router and we will take a look at a NetFlow server.

![Pasted image 20240527135759](https://github.com/lm3nitro/Projects/assets/55665256/96eff4b5-f78f-44fb-9da0-9648f9f76d12)



Booting up:

![Pasted image 20240527105109](https://github.com/lm3nitro/Projects/assets/55665256/7173568f-5a92-42d8-a8a9-59c0e7ce0ee6)


![Pasted image 20240527105321](https://github.com/lm3nitro/Projects/assets/55665256/1f494a16-2834-443d-83f7-6ca187c8307c)


Configuration:

![Pasted image 20240527111454](https://github.com/lm3nitro/Projects/assets/55665256/1ed26cdd-895f-4ddc-a6d5-3289d1ac8b0e)



Configuring DHCP for  ***LAN_NETWORK***

![Pasted image 20240527123457](https://github.com/lm3nitro/Projects/assets/55665256/1e281444-bd5a-4e38-ad2d-770079e786de)



Client side DHCP traffic:


![Pasted image 20240527112902](https://github.com/lm3nitro/Projects/assets/55665256/75042c71-8b57-4c70-9eaa-ca156fd1f311)


On the client:

![Pasted image 20240527113026](https://github.com/lm3nitro/Projects/assets/55665256/ab4ee537-c7b4-4eee-9822-6cf29fc4f216)




## Displaying a specific DHCP active lease, or all active leases: 

show ip dhcp binding

![Pasted image 20240528081836](https://github.com/lm3nitro/Projects/assets/55665256/ce97d6d4-f03d-448e-8cbf-3dc2b59df3bf)



NetFlow configuration:

First we have to create a NetFlow profile to assign the IP of the NetFlow collector and the source of the interface we wan to collect from as well as the port


flow exporter lm3nitro_netflow
 destination 172.16.10.10
 source GigabitEthernet0/0
 transport udp 9996


Then we have to configure a monitor interface:

flow monitor lm3nitro_monitor
 exporter lm3nitro_netflow
 record netflow ipv4 original-input


Output from the show running configuration :

![Pasted image 20240527114210](https://github.com/lm3nitro/Projects/assets/55665256/180b9ced-7c5c-4004-8fc9-fdd5262647ed)


Verification :


![Pasted image 20240527113517](https://github.com/lm3nitro/Projects/assets/55665256/8cc96c80-5a25-42e4-a2e7-a571a27fa815)


![Pasted image 20240527114022](https://github.com/lm3nitro/Projects/assets/55665256/9dfd4fc2-e466-4a40-8120-67fff92186a8)


Applying the monitor NetFlow profile called "lm3nitro_monitor" to an interface:


Note: The command needs to be apply on the actual interface.

ip flow monitor lm3nitro_monitor input

![Pasted image 20240527115028](https://github.com/lm3nitro/Projects/assets/55665256/51c77bc4-8b13-4aab-880c-8fc2a628f601)



Let's look at information about the flow records that we have, at the moment we don't cache entry:



show flow monitor name lm3nitro_monitor cache

![Pasted image 20240527115445](https://github.com/lm3nitro/Projects/assets/55665256/757308da-c6d4-4594-8946-e88f7a489824)


Let's generate some traffic using Zenmap:

Note:

The router is getting DHCP from  a network 172.16.1.0/24 , I'm generating traffic to the Firewall gateway 10.10.100.1 which is located on the "gi0/1 WAN_INTERFACE" Also, Zemmap won't be able to pick up any information from the firewall because it's blocking all the broves:




![Pasted image 20240527120720](https://github.com/lm3nitro/Projects/assets/55665256/1bb007ee-1270-4c1f-b9d1-41d047f78376)


Zemmap:



![Pasted image 20240527120501](https://github.com/lm3nitro/Projects/assets/55665256/769e45d1-db83-410b-9443-227c6b1313cc)




Let's check the NetFlow cache again:

![Pasted image 20240527120624](https://github.com/lm3nitro/Projects/assets/55665256/680eb29d-8e94-41fe-aba6-386b056b7978)


NetFlow traffic sent to NetFlow collector by the router:

![Pasted image 20240527121713](https://github.com/lm3nitro/Projects/assets/55665256/45b46952-c21f-4d59-9a0f-5864083b190a)



Details of the flows:

![Pasted image 20240527121941](https://github.com/lm3nitro/Projects/assets/55665256/b466ac64-246b-4348-8b4f-a97f0ed9fc82)





Configurating NAT/PAT on the Cisco Router:


We have to select the interfaces that are outside and inside.



interface gi1/0
  ip nat outside

interface gi0/0
  ip nat inside


![Pasted image 20240527125004](https://github.com/lm3nitro/Projects/assets/55665256/856395a8-d067-4cea-980f-bf15adb65be9)



Creating a access list to allow traffic to the translated:

ip nat inside source list 1 interface GigabitEthernet0/1 overload
ip route 0.0.0.0 0.0.0.0 172.16.1.1


![Pasted image 20240527124004](https://github.com/lm3nitro/Projects/assets/55665256/85fb07e3-a36e-4f21-a0ce-cd824def2848)


 Creating a static route pointing to  Firewall on172.16.1.1:
 
![Pasted image 20240527124747](https://github.com/lm3nitro/Projects/assets/55665256/99fe00e8-4d25-4f1c-8e95-c8e6ea251282)




Let's look at the NAT translations:


sh ip nat translations

![Pasted image 20240527124225](https://github.com/lm3nitro/Projects/assets/55665256/f2b76dce-f920-4042-9c7c-e7bbd2ac084f)



Let's look at our Netflow cache:


We have new cache entries:

![Pasted image 20240527125355](https://github.com/lm3nitro/Projects/assets/55665256/794f47d6-b83d-4482-bbe0-0e0e21bfbdb4)




Tracking flows on Gi0/0 interface:


Note:

The ip route-cache flow command  will track all ingress flows on the physical and all sub-interfaces. 

You can also use the ip flow egress or ip flow ingress commands if you only want to enable it on one sub-interface or in one direction.


![Pasted image 20240527132357](https://github.com/lm3nitro/Projects/assets/55665256/cd41312d-f79c-46bb-85f4-43c188dba955)


Let's generate more traffic:


![Pasted image 20240527132654](https://github.com/lm3nitro/Projects/assets/55665256/f2683e91-cd15-4b7b-96c9-3f3224c3ff68)




# Tunning "NetFlow":


This is optional configuration:


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
