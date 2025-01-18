# Netgear Span Port

<img width="261" alt="Screenshot 2024-04-27 at 11 31 37 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b47f1e79-e323-4f98-8a36-5d310a6dab40">

### Scope: 

In this exercise, I will be configuring port mirroring on a newly purchsed Netgear switch to capture packets, this involves setting up a mechanism that mirrors the traffic from specific ports to a designated "monitoring" or "capture" port. This will allow me to capture and analyze the network traffic that passes through the source ports without affecting the normal operation of the network.

### Tools and Technology:
Netgear GS108Ev3 switch

Topology:

![Pasted image 20240416134504](https://github.com/lm3nitro/Projects/assets/55665256/c5c73a32-7099-4973-8ba3-496506054882)

This is the interface I was presented with upon logging into the switch. First, I disabled DHCP:

<img width="1501" alt="Screenshot 2025-01-18 at 4 09 25 PM" src="https://github.com/user-attachments/assets/be872b2c-4377-4ce7-83f6-f17565597130" />

I then navigated to System > Monitoring> Mirroring. Here I can choose the port I want for port mirroring: 

![Pasted image 20240416110801](https://github.com/lm3nitro/Projects/assets/55665256/0f8a18c5-5d5a-4410-9d2f-907cc9bef7f6)

After, I went to VLAN > Basic. Here I enabled it:

![Pasted image 20240416111143](https://github.com/lm3nitro/Projects/assets/55665256/9b682e1c-d6c7-44e9-8a19-332694f50943)

Once enabled, I was presented with a list of available ports, allowing me to choose which ports will belong to which VLAN.

![Pasted image 20240416111318](https://github.com/lm3nitro/Projects/assets/55665256/3e7f972e-7ad3-4ba3-92d2-f5bf7e31784e)

This is the Switch itself and which ports I configured as the source port and the mirroring port:

![Pasted image 20240416131928](https://github.com/lm3nitro/Projects/assets/55665256/1771622d-31ce-46fa-9e07-dff438489204)

### Summary:

In this lab, I configured port mirroring on my Netgear switch, which will allow me to capture traffic from my host. This setup helps observe network behavior, troubleshoot issues, and analyze performance without interrupting the flow of data on the network. Key takeaways include:

+ Learning how to set up port mirroring to monitor specific network segments
+ Understanding the importance of traffic analysis in network management
+ Recognizing the role of the monitoring port in security and performance diagnostics
  
The next phase of the lab, configuring a sniffing interface, will build on this by enabling real-time packet capture and analysis, providing further insight into network traffic patterns and potential vulnerabilities.



