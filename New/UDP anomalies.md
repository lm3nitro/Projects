

**Scenario: UDP traffic on Non-Standard Port**


Network monitoring system detects a significant increase in UDP traffic on a non-standard port (e.g., UDP port  12345) that is not typically used for legitimate purposes within your network.

![Pasted image 20240411124409](https://github.com/user-attachments/assets/b0298557-802f-4954-bf61-2dacfe31d1a3)


### Suricata flow Traffic in Splunk: 


![Pasted image 20240411105417](https://github.com/user-attachments/assets/e92bac7f-18c6-481a-ae6c-c08e95ba0b9f)


![Pasted image 20241008105558](https://github.com/user-attachments/assets/75aa79fc-e07c-40f5-b499-a73b99230f47)


Taking a live packet capture with Tcpdump to analyse UDP traffic  that deviate from normal behaviour going to port 12345:


![Pasted image 20240411121826](https://github.com/user-attachments/assets/1b9ff9d7-fd70-4cd7-8079-fbe7f8f39e83)


Note: The length of packet is changing.
## Payload analysis with TCPDump:

Creating a filter in  to capture payload over 64 bytes in length:


![Pasted image 20240411121126](https://github.com/user-attachments/assets/8b1edf11-77a1-42c7-8231-4c4397973709)





![Pasted image 20241008105430](https://github.com/user-attachments/assets/c8da32f4-c857-48c2-b5c1-95459e972828)

## Packet Lengths statistics with Wireshark:



![Pasted image 20240411121542](https://github.com/user-attachments/assets/525f5700-0156-424f-8e3f-6e378de8add6)



Monitor network traffic for unusual patterns or unexpected UDP traffic on non-standard ports. It is important to Investigate devices and systems to determine if unauthorized applications are generating UDP traffic.


Looking at conversations statistics: 



![Pasted image 20240411114316](https://github.com/user-attachments/assets/78ba84ae-3127-4808-97b0-6831809506b9)



Attackers may exploit UDP traffic without clear application identification to spoof protocols or disguise malicious activities such as tunneling traffic over non-standard ports.

 Looking at protocol hierarchy:


![Pasted image 20240411114359](https://github.com/user-attachments/assets/4ad1fae0-2b08-4253-8d53-9d2a8e3280b8)



Protocol spoofing and tunneling techniques can bypass network security measures, evade detection, and facilitate unauthorized data exfiltration, command and control (C2) communication, or lateral movement within the network.



![Pasted image 20240411130628](https://github.com/user-attachments/assets/f3758be3-3f03-46ee-9d95-e3fa07e289eb)





# Scenario: UDP Flood DDoS Attack:


### Objective: Overwhelm the server's resources to disrupt services for legitimate users.


#### CPU utilization :

![Pasted image 20241008104703](https://github.com/user-attachments/assets/5b8eda41-f52c-4a27-a67a-00f246447b00)


### Network card utilization: 


![Pasted image 20241008105009](https://github.com/user-attachments/assets/575fd586-4c87-4579-b910-edb64f2ed889)


### Detecting UDP flood attacks:

High Volume of UDP Traffic: 
An unusually high volume of UDP packets, exceeding normal thresholds.

Random Source IP Addresses: 

A large number of UDP packets coming from a wide range of random source IPs, with similar destination ports and IP





![Pasted image 20241008105416](https://github.com/user-attachments/assets/453ec337-3b9d-4d22-8682-7cec51bc35c8)



### Protocol hierarchy:


![Pasted image 20240411161413](https://github.com/user-attachments/assets/c3901459-1674-4c08-a58f-b115ba6cd569)

UDP DDoS (Distributed Denial of Service) attacks leverage the User Datagram Protocol (UDP) to flood a target with a high volume of traffic, overwhelming it and disrupting its normal operations. 

### Conversations:


![Pasted image 20240411153412](https://github.com/user-attachments/assets/eb61faf0-0267-4e62-925b-be2d12fbde74)




### Wireshark expert information: 


![Pasted image 20240411153123](https://github.com/user-attachments/assets/acd56d34-be49-491d-9911-b089b368db06)


### Visualizing packet length:



![Pasted image 20240411154027](https://github.com/user-attachments/assets/74aac863-fdfb-4c20-aa0c-2b5c62d2c3d1)


### Taking a close look to average of the packets:


![Pasted image 20240411152810](https://github.com/user-attachments/assets/a83c6897-072e-462c-b86d-3374eae8a7c6)



## Payload analysis:



![Pasted image 20240411153934](https://github.com/user-attachments/assets/5ddc32d1-68a9-4a12-aff3-dd324f3f9aaa)


In a UDP flood attack, the attacker sends a large number of UDP packets to random or specific ports on the target server. This can exhaust the target's resources as it tries to process the incoming packets and respond, leading to service degradation or outages.
### Mitigation:

Implementing rate limiting on the  attacked server can help mitigate the effects of a UDP DDoS attack and reduce the load during an attack.
