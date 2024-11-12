# Macof

Macof, short for “Mac Flooding", is a tool from the dsniff suite used for generating random MAC addresses and flooding a network with Ethernet frames. It can overwhelm a switch's MAC address table, causing it to behave like a hub and broadcast all traffic, which is useful for testing network resilience and security assessments. 

![Pasted image 20241011131110](https://github.com/user-attachments/assets/a5eb6087-1388-46b2-8406-6e64dad4fe97)

### Scope:

In my lab, I have a Cisco router and switch, and a Kali Linux host to test the behavior of the switch under stress using macof. I will utilize macof to generate a high volume of random MAC addresses, flooding the network with Ethernet frames. As the switch's MAC address table fills up, I will be capturing the traffic using Wireshark to analyze it and see the impact of MAC flooding on network performance. I will then implement a mitigation technique and try the test again to see if I am successful in blocking this type of attack. 

IP address information for this scenario:

Router IP: 192.168.91.216
Kali Linux : 192.168.128

### Tools and Technology:

Linux, Cisco router, Cisco switch, and Wireshark

## Kali Linux:

To get started, I verified information in my Kali Linux host, checking the IP, connectivity, and link layer information. 

Kali Linux Host TCP/IP information:

![Pasted image 20241011113823](https://github.com/user-attachments/assets/593c0ade-3dd9-411f-b1a0-4ddab051d091)

Reachability test:

![Pasted image 20241011113842](https://github.com/user-attachments/assets/847cc790-cb0b-4d1d-929f-5491329c96f9)

Link layer information:

![Pasted image 20241011113908](https://github.com/user-attachments/assets/7c80144d-4e90-4f9c-9000-ddf9270387a0)

## Switch Information:

I then checked the switch interface and verified that the interface was up:

```
show interfaces gigabitEthernet 0/1 switchport
```

![Pasted image 20241011111411](https://github.com/user-attachments/assets/3d0185af-8bd8-4b2a-bd6d-69889a7d901b)

```
show interfaces gigabitEthernet 0/1
```

![Pasted image 20241011111458](https://github.com/user-attachments/assets/38acfa81-0ff3-4a23-9643-43511e2b5dc0)

I also checked and confirmed the MAC address table prior to using macof:

```
show mac address-table interface gigabitEthernet 0/1
```

![Pasted image 20241011111839](https://github.com/user-attachments/assets/70a16751-55cc-4ac1-9fe9-208c9bec7cb8)

## MAC Flooding:

After verifying that everything was configured and set to go, I then started the MAC flooding attack. 

```
macof -i eth0 -n 50
```

![Pasted image 20241011115206](https://github.com/user-attachments/assets/46b20018-a3a6-4e09-9d5c-f5737f577a11)

Afer performing the attack, I went back to check the MAC address table:

![Pasted image 20241011114622](https://github.com/user-attachments/assets/c95bc533-50a3-44b7-93e2-840d2a5d6c5e)

## Traffic Analysis:

Utilizing Wireshark, I also captured the traffic while performing that attack. Looking at the traffic, I can see the ARP protocol behavior:

![Pasted image 20241011121731](https://github.com/user-attachments/assets/c6148419-96c6-4b99-ab8d-a81da1db5387)

I was also able to see the random source and destination IPs as well MAC addresses: 

![Pasted image 20241011114906](https://github.com/user-attachments/assets/0fe11941-cd73-44f0-a1af-f77b0d8eb7f2)

## Mitigation

Implementing port security:

Port security is a network security feature on switches that helps control access to a network by limiting the number of MAC addresses that can connect to a specific port. It allows network administrators to specify how many devices can be connected to each port and can even remember the MAC addresses of those devices.

Configuration:

```
enable
configure terminal
interface GigabitEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
end
write memory
```
```
sh interfaces status
```

After configuring the switch with port security, I then attempted to perform the MACA flooding again, however this time I was able to see the switch and now the interface is disabled:

![Pasted image 20241011120604](https://github.com/user-attachments/assets/7104be41-9532-4472-990f-316467f74eb4)

Policy violation error:

![Pasted image 20241011120341](https://github.com/user-attachments/assets/0c097480-9c2c-4047-b05e-9ad02ddce9c7)

Taking a closer look at the interface details:

```
sh interfaces gigabitEthernet 0/1
```
The interface is down due to the port security violation:

![Pasted image 20241011120431](https://github.com/user-attachments/assets/bb453da3-95c2-4514-a29f-f6d7b5b71975)

```
sh mac address-table  interface gi0/1
```

![Pasted image 20241011120705](https://github.com/user-attachments/assets/8b1b2e61-8325-41d4-aaa0-a269f9be5808)

The mitigation was successful, and the MAC flooding was prevented.

### Summary:

In this lab, I was able to perform a MAC flooding attack while analyzing the traffic in Wireshark. One of the behaviors observed is the significant increase in ARP traffic, particularly those with varying source MAC addresses. This helps confirm that a MAC flooding attack is occurring. I was also able to observe a high turnover rate of MAC addresses, indicating that the switch was being overwhelmed. To mitigate this, I implemented port security and simulating the attack again to ensure that the security features functioned effectively in preventing unauthorized access and protecting the network.

Mitigation:

1. Implement Port Security
   + Configure port security on switches to restrict the number of MAC addresses learned on each port.
   + Use sticky MAC address configurations, which allow the switch to remember dynamically learned MAC addresses and retain them even after a reboot.
2. Use VLAN Segmentation
   + Segment the network into multiple VLANs to limit the broadcast domain. This helps contain any potential MAC flooding to a smaller area of the network, reducing overall impact.
3. Rate Limiting
   + Configure rate limiting on switch ports to control the amount of traffic that can be sent to and from a port. This can help prevent overwhelming the switch with excessive MAC address learning.
4. Network Monitoring and Logging
   + Implement comprehensive monitoring to capture network traffic and log switch performance. Regularly review logs for unusual MAC address learning rates or other signs of potential flooding.

