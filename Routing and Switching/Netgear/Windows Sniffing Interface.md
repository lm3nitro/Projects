# Windows Sniffing Interface

### Scope: 
The scope of this exercise is to configure a sniffing interface on a Windows host to capture and analyze network traffic. The goal is to set up a network interface in "promiscuous mode" so that it can intercept all packets. This configuration will enable the use of packet analysis tools, such as Wireshark, to inspect traffic for troubleshooting, security analysis, and performance monitoring. 

## Getting Started:
This is the hardware information for the device I will be configuring:

![Pasted image 20240416122451](https://github.com/lm3nitro/Projects/assets/55665256/fa3772e5-ae40-4631-b5f3-049edb89c591)

This is the NIC that the device has, it is an Intel Ethernet Server Adapter I350

![Pasted image 20240416120704](https://github.com/lm3nitro/Projects/assets/55665256/f09093dd-75d3-4a08-8b44-057d1aea4036)

To get started, I navigated to the ethernet interface I will be configuring. In my case it is ethernet 3:

![Pasted image 20240416113418](https://github.com/lm3nitro/Projects/assets/55665256/2085cecb-b86a-4b3d-9ebf-3bc21a58bf44)

Disabling Checksum Offload:

1. Open the Network Connections page of the Windows Control Panel.
2. Open the "Properties" dialog of the NIC.
3. Select "Configure..." to open the NIC's configuration.
4. Navigate to the Advanced tab and disable TCP and UDP Checksum Offload for BOTH IPv4 and IPv6.

![Pasted image 20240416114153](https://github.com/lm3nitro/Projects/assets/55665256/59a1b14c-d419-49c9-9437-a5502392ac8b)

The Internet checksum, also called the IPv4 header checksum is a checksum used in version 4 of the Internet Protocol (IPv4) to **detect corruption in the header of IPv4 packets**. It is carried in the IP packet header, and represents the 16-bit result of summation of the header words.

![Pasted image 20240416115911](https://github.com/lm3nitro/Projects/assets/55665256/5398b12f-a2d0-400d-9e0d-d1937e8a9595)

RSS is a host NIC technique that **allows spreading of receive traffic among many receive paths, each on a dedicated CPU**. RSS allows network traffic to scale with number of CPU used to achieve better traffic characteristics, lower latencies and/or higher throughput.

![Pasted image 20240416114407](https://github.com/lm3nitro/Projects/assets/55665256/160cf6e6-fd64-42ef-8497-61f8230cf2fa)

TCP offloading **sends some network packets directly to the NIC for processing**. It skips the operating system's network stack and prevents DAM's packet capture driver from seeing these packets. This feature is a normal feature of many operating systems, including Windows and Linux

![Pasted image 20240416132512](https://github.com/lm3nitro/Projects/assets/55665256/e02ecf22-6afd-468f-864c-b23836de6b80)

![Pasted image 20240416112747](https://github.com/lm3nitro/Projects/assets/55665256/6eab40c1-e808-4c88-8083-dc946253fa41)

>#### Note: Only Npcap Packet Driver  (NPCAP) should be enable on the interface because  it will be use for Wireshark or dumpcap to sniff traffic on the wired

![Pasted image 20240416112832](https://github.com/lm3nitro/Projects/assets/55665256/f12f209c-c51b-480d-b6c0-b246a0df1721)

Npcap is an architecture for **packet capture and network analysis for Windows operating systems**, consisting of a software library and a network driver.

Enabling promiscuous mode in Wireshark:

![Pasted image 20240416121653](https://github.com/lm3nitro/Projects/assets/55665256/08f23dca-6854-41a2-b0ff-50bdc71c08d3)

To test, I captured some traffic going to chess.com:

```
frame contains "chess.com"
```

![Pasted image 20240416122239](https://github.com/lm3nitro/Projects/assets/55665256/43deb300-4bcb-41b3-8788-fe975d6aeb56)

### Summary:

In the exercise, I was able to configure a sniffing interface on my Windows host. This allows for in-depth network traffic analysis, which is crucial for diagnosing connectivity issues and identifying performance bottlenecks. By capturing and analyzing this traffic data, I will be able to monitor all the traffic from this host and gain insights into the communication between devices on the network. This will allow me to identify any potential network issues, such as slow response times, packet loss, or misconfigurations, and take corrective actions. 
