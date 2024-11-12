# Detecting Abnormal ICMP Traffic

## First Scenario:

You are a network security analyst tasked with investigating suspicious activity on a network. The network monitoring system has flagged a series of ICMP packets as potentially malicious, and you have been provided with a Packet Capture (PCAP) file containing the captured network traffic for analysis.

Step 1:

Verify the pcap information with Capinfos:
```
capinfos threat_actor.pcap
```
**Capinfos** is a program that reads one or more capture files and returns some or all available statistics (infos) of each <_infile_> in one of two types of output formats: long or table.

![Pasted image 20240324111115](https://github.com/lm3nitro/Projects/assets/55665256/3afe59b6-d7e7-4868-adb2-571ba9d15d53)

Step 2:

In this Packet Capture (PCAP) file, I can see that there are 544 packets captured. I will need to filter those packets in order to identify if there is any abnormal activity. 

Filter for the number of ICMP packets:
```
tcpdump -nr threat_actor.pcap icmp | wc -l
```
![Pasted image 20240324112004](https://github.com/lm3nitro/Projects/assets/55665256/5d960c59-e605-4528-9c0f-f4a29fe5b3c2)

Note: Normal ICMP packets are usually 64 bytes data in length, and they have incremental ASCII characters values. 

>###Example:

```
tcpdump -nr normal_icmp.pcap -ttttnnvvA
```

![Pasted image 20240324120252](https://github.com/lm3nitro/Projects/assets/55665256/49788d58-5421-4845-b7df-621757143e3c)

Since I know that normal ICMP are normally 64 bytes of data in length, I filtered for ICMP packets that are over "64" bytes in length to detect suspicious ICMP behavior:

```
tcpdump --number -ttttnr threat_actor.pcap icmp and greater 100
```

![Pasted image 20240324122350](https://github.com/lm3nitro/Projects/assets/55665256/926eaefb-50ff-4c96-8e22-95e8f2e2f2cb)

Step 3:

I then took a closer look at one of packets where I could see that it was larger than the expected 64 bytes of data. 

```
tcpdump --number -ttttnr threat_actor.pcap icmp and greater 100 -c 1 -X
```

![Pasted image 20240324123214](https://github.com/lm3nitro/Projects/assets/55665256/493a02df-75ca-4681-8f8e-c75320c14687)

Analysis: Upon investigation of the network traffic in the Packet Capture (PCAP) file, I discovered an ICMP tunnel. This indicates that the threat actor is utilizing an encrypted SSH tunnel and leveraging the ICMP protocol to conceal their communication. This technique is commonly employed for purposes such as data exfiltration or command and control operations. An ICMP tunnel involves encapsulating data within ICMP packets, allowing the attacker to bypass traditional network security measures that may not inspect ICMP traffic thoroughly. By using an encrypted SSH tunnel, the attacker adds an additional layer of security to obfuscate the contents of the communication, making it more challenging to detect and analyze.


## Second Scenario

The network monitoring system has flagged a series of ICMP packets due to a significant increase in ICMP traffic originating from a specific IP address. You have been provided with a Packet Capture (PCAP) file containing the captured network traffic for analysis.

Step 1: 

In this scenario, my objective is to identify any indications of reconnaissance activity by the threat actor using ICMP on my network 192.168.11.0/24

```
tcpdump -ttttnr threat_actor.pcap icmp and net 192.168.11.0/24
```

![Pasted image 20240324134231](https://github.com/lm3nitro/Projects/assets/55665256/3068949f-c8ac-4078-924f-411d35e0b45a)

From the output, it's evident that the IP address 92.242.140.21 is actively sending ICMP echo requests to multiple hosts within my network within a very short time period. This rapid succession of requests is indicative of potential scanning or probing activities aimed at identifying vulnerabilities or mapping network topology.

Step 2:

Next, I proceeded to inspect the payload to check for any further insights or malicious content.

```
tcpdump -ttttnr threat_actor.pcap icmp and net 192.168.11.0/24 -c 1 X
```

![Pasted image 20240324135225](https://github.com/lm3nitro/Projects/assets/55665256/10aa77d8-a801-4330-ad75-1c17a866d233)

Analysis: When taking a look at the payload of one of the ICMP packets, I could see a pattern. Patterns such as sequential or incrementing data are common in scanning tools. Key indicators, as previously mentioned include a high volume of ICMP echo (ping) requests sent from a single source IP address to multiple destination IP addresses within a short time frame. 

ICMP echo request (ping) scans, serve the purpose of gathering reconnaissance information about a target network or host. Malicious actors use ICMP scans to identify live hosts by sending ICMP echo requests to various IP addresses. This allows them to map the network topology, detect the presence of firewalls or filtering devices, measure round-trip times (RTT), and potentially identify vulnerable systems based on ICMP responses. Ultimately, ICMP scans provide attackers with crucial insights into the network's structure, active hosts, and potential weaknesses, enabling them to plan and execute targeted attacks more effectively.

## Third Scenario

One evening, the network monitoring system at detected unusual activity originating from an internal server within the corporate network. The activity included a significant increase in ICMP (Internet Control Message Protocol) traffic, specifically ICMP echo (ping) requests, targeting multiple internal IP addresses. Let's analyze the pcap that was provided.  

```
tcpdump -nr ping_sweep.pcap icmp and less 64 -c 20
```

![Pasted image 20240324131438](https://github.com/lm3nitro/Projects/assets/55665256/3eae4d30-ef1a-43b5-8a19-9edc817d10a7)

I can see here ICMP traffic that has several indicators that show that is is not normal bahvior. Let's take a look at the payload:

```
tcpdump -nr ping_sweep.pcap icmp and less 64 -c 1 -XX
```

![Pasted image 20240324132714](https://github.com/lm3nitro/Projects/assets/55665256/7ec999ae-60f1-4376-ade9-f40ecd716a3a)

Analysis: The first things that I saw was the order of sequential probes [1, 2, 3...23]. The IP identification in this case is also changing per request, but the length of the ICMP payload is 8; normal ICMP should be 64. Additionally, the speed of each request is lightning-fast. Not to mention that the payload does not contain any incremental values; it looks very suspicious.

## Fourth Scenario

This was another generated alert in the network for ICMP traffic in our network. Let's investigate why the alert was triggered. 

First, let's take a look at the echo requests:
```
tcpdump -nr icmp_attacks.pcap "icmp[0]==8" and ! net 192.168.0.0/16
```
![Pasted image 20240324171440](https://github.com/lm3nitro/Projects/assets/55665256/03a3650b-8666-4ee6-b5e6-f9d0f79144be)

Here I can see that again the length of the ICMP requests is only 8, and the packets are coming in very fast. Now, let's take a look at the responses.

```
tcpdump -nr icmp_attacks.pcap "icmp[0]==8" and ! net 192.168.0.0/16
``` 
![Pasted image 20240324172031](https://github.com/lm3nitro/Projects/assets/55665256/7807bb0b-f4d3-4bd4-a8b9-28894ac34055)

Next, let's take a look at the TTL value of the source IP address. TTL values in network packets indicate the maximum number of hops they can traverse before being discarded. Analyzing TTL values helps determine packet routes, estimate network distances, and identify anomalies or potential security issues. It can also provide information regarding the OS.  

![Pasted image 20240324172644](https://github.com/lm3nitro/Projects/assets/55665256/940955ad-bf56-4ff9-a18c-2af5a6049041)

Analysis: The number of ICMP requests coming from different sources at the same time with the same TTL value, data length, and IP identification ID suggests that the sources are not passing through a single router. Additionally, the fact that public IP ranges are sending traffic to an internal host indicates that the host is behind NAT or that a threat actor is spoofing the source IP address to evade detection. 

### Mitigation:
To mitigate the risk of ICMP tunneling, scanning, or spoofing, consider disabling ICMP echo requests (ping) at the network perimeter to prevent external probing. Additionally, configure firewall rules to block ICMP traffic that is not essential for network operations. Regularly monitor and analyze ICMP activity to detect any unauthorized or suspicious usage.



