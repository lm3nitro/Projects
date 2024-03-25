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
In this Packet Capture (PCAP) file, we can see that there are 544 packets captured. We will need to filter those packets in order to identify if there is any abnormal activity. 

Filter for the number of ICMP packets:
```
tcpdump -nr threat_actor.pcap icmp | wc -l
```
![Pasted image 20240324112004](https://github.com/lm3nitro/Projects/assets/55665256/5d960c59-e605-4528-9c0f-f4a29fe5b3c2)

Note: Normal ICMP packets are usually 64 bytes data in length and they have incremental ASCII characters values. 

>###Example:

```
tcpdump -nr normal_icmp.pcap -ttttnnvvA
```

![Pasted image 20240324120252](https://github.com/lm3nitro/Projects/assets/55665256/49788d58-5421-4845-b7df-621757143e3c)

Since we know that normal ICMP are normally 64 bytes of data in length, we will want to filter for ICMP packets that are over "64" bytes in length to detect suspicious ICMP behavior:

```
tcpdump --number -ttttnr threat_actor.pcap icmp and greater 100
```

![Pasted image 20240324122350](https://github.com/lm3nitro/Projects/assets/55665256/926eaefb-50ff-4c96-8e22-95e8f2e2f2cb)

Step 3:

We will then want to take a closer look at one of packets where we can see that it is larger than the expected 64 bytes of data. 

```
tcpdump --number -ttttnr threat_actor.pcap icmp and greater 100 -c 1 -X
```

![Pasted image 20240324123214](https://github.com/lm3nitro/Projects/assets/55665256/493a02df-75ca-4681-8f8e-c75320c14687)

Upon analysis of the network traffic in the Packet Capture (PCAP) file, we discovered an ICMP tunnel. This indicates that the threat actor is utilizing an encrypted SSH tunnel and leveraging the ICMP protocol to conceal their communication. This technique is commonly employed for purposes such as data exfiltration or command and control operations.

An ICMP tunnel involves encapsulating data within ICMP packets, allowing the attacker to bypass traditional network security measures that may not inspect ICMP traffic thoroughly. By using an encrypted SSH tunnel, the attacker adds an additional layer of security to obfuscate the contents of the communication, making it more challenging to detect and analyze.

The combination of an ICMP tunnel and encrypted SSH communication suggests a sophisticated approach by the threat actor to disguise their malicious activities and evade detection. This technique is often associated with advanced threat actors and targeted attacks aimed at compromising sensitive data or gaining unauthorized access to systems.


### Metigation:
To mitigate the risk of ICMP tunneling, consider disabling ICMP echo requests (ping) at the network perimeter to prevent external probing. Additionally, configure firewall rules to block ICMP traffic that is not essential for network operations. Regularly monitor and analyze ICMP activity to detect any unauthorized or suspicious usage.
Identifying ICMP scanning:

In this scenario we want to detect for any sings of reconesence  from the threat actor over ICMP to our network 192.168.11.0/24

![Pasted image 20240324134231](https://github.com/lm3nitro/Projects/assets/55665256/3068949f-c8ac-4078-924f-411d35e0b45a)

Payload inspection:

![Pasted image 20240324135225](https://github.com/lm3nitro/Projects/assets/55665256/10aa77d8-a801-4330-ad75-1c17a866d233)


Note:  The indicator  or sings of  scanning are:

Even though the payload looks like a leyement one  the key factor are order of  sequeial proving [1,2,3,4,10] . The IP identification is the same, also the speed of each request is lighting fast


In this scenario we're to looking for any sings of ICMP estrange behaviour to network 172.16.1.0/24:  

![Pasted image 20240324131438](https://github.com/lm3nitro/Projects/assets/55665256/3eae4d30-ef1a-43b5-8a19-9edc817d10a7)

Payload inspection:

![Pasted image 20240324132714](https://github.com/lm3nitro/Projects/assets/55665256/7ec999ae-60f1-4376-ade9-f40ecd716a3a)

Note:  The indicators are:

The order of  sequeial probes [1,2,3..23] . The IP identification in this case are chaing per request but the lenght of the ICMP payload is 8 , normal ICMP should be 64 also the speed of each request is lighting fast not to mention that the payload  does not contain any incremental values inside, it looks very suspicious. 


Identify ICMP scanning spoofing random source:

Filtering for echo request:

![Pasted image 20240324171440](https://github.com/lm3nitro/Projects/assets/55665256/03a3650b-8666-4ee6-b5e6-f9d0f79144be)

Filtering for echo Reponses: 

![Pasted image 20240324172031](https://github.com/lm3nitro/Projects/assets/55665256/7807bb0b-f4d3-4bd4-a8b9-28894ac34055)

Analysing the TTL value of the source address

![Pasted image 20240324172644](https://github.com/lm3nitro/Projects/assets/55665256/940955ad-bf56-4ff9-a18c-2af5a6049041)

Note: The mount of ICMP request coming from different sources at the same time with the same TTL value, data length and the IP identification ID. It seems like all the source are  not even passing trough  a single router.  Also the fact that public IP ranges are sending traffic to  an internal host would mean that the host is behind the NAT or the a threat actor is spoofing the source IP address to evade detection. It's very suspicious. 




