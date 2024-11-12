# DNS Tunneling

DNS tunneling is a difficult-to-detect attack that routes DNS requests to the attacker's server, providing attackers a covert command and control channel, and data exfiltration path. DNS tunneling is a technique that encapsulates data within DNS queries and responses, allowing data to be sent and received through DNS traffic. This method can be used for both legitimate purposes, such as bypassing network restrictions, and malicious activities, such as exfiltrating data or establishing command-and-control channels for malware.

### Scope:

While monitoring network traffic in Kibana, I noticed an unusual spike in DNS queries from a single internal IP address, which significantly exceeded the normal baseline. This anomaly raised concerns about potential DNS tunneling or unauthorized data exfiltration. To investigate further, I drilled down into the logs and network traffic, examining the specific DNS queries being made, the domains being accessed, and the timestamps of the requests. I also cross-referenced this data with user activity logs to identify any recent changes or suspicious behavior associated with that IP address, aiming to determine whether this spike was a benign issue, such as a misconfigured application, or a sign of a more serious security threat.

**Topology:**

![Pasted image 20241008094034](https://github.com/user-attachments/assets/66dbf3d6-4dad-4bb3-9a34-5987dc8d3b12)

### Tools and Technology:

DNS, Kibana, Linux, Rita, Zeek, Tshark, and Wireshark

## Getting Started

While monitoring network traffic with Kibana I was able to see a sudden spike in DNS:

![Pasted image 20240416141836](https://github.com/user-attachments/assets/7708a48e-4dd2-4624-bfef-d490d49bc7a7)

> [!TIP]
> There are factors that help identify and determine a DNS tunnelling attack:
> + Long Domain Names: DNS queries with unusually long lengths with more than 30 characters.
> + Frequent Queries: High frequency of requests to the same domain over a short period.
> + Non-Standard Characters: Domains that include special characters or seem random.

## Pcap Analysis

Next, I took a look at the pcap that was running when the DNS spike occurred. I used capinfos to view all the capture information such as size, duration, number of packets, etc. Here I see that the pcap ran for 24 hours:

![Pasted image 20241008083359](https://github.com/user-attachments/assets/35e21ed1-5491-43e4-8b0e-fc871d6f4c0e)

I then used zeek to parse through the pcap:

```
zeek readpcap dns_tunneling.pcap
```
Zeek is an open-source network security monitoring tool that analyzes network traffic in real-time and can also process packet capture (PCAP) files. It provides deep visibility into network activities and allows users to create custom scripts for specific monitoring needs.

Here is the view and how the CPU went up while it was processing:

![Pasted image 20241008083742](https://github.com/user-attachments/assets/77942bb2-a075-45b8-86d8-27d2bb296462)

These are two files with the highest amount of data: `conn.log` and `dns.log`

![Pasted image 20241008093150](https://github.com/user-attachments/assets/c9c83bf3-ed9e-4de9-9378-a553a69be8af)

## Rita:

Rita is an open-source tool designed to analyze Zeek's logs and provide insights into potential security threats. It focuses on detecting anomalies and providing actionable intelligence to improve network security posture.

![Pasted image 20241008092949](https://github.com/user-attachments/assets/0abb7e95-37c0-4fda-a523-57c4ae6ee3d9)

I then imported the Zeek logs generated above into Rita:

```
rita import -l /opt/zeek/manual-logs/ -d hunting_2
```

![Pasted image 20241008084930](https://github.com/user-attachments/assets/a587c7c2-8124-4774-b41a-98b80b575427)

To see the analysis Rita performed, you can view the datasets with the following command:

```
rita list
```

![Pasted image 20241008085120](https://github.com/user-attachments/assets/ae22dcce-0fdd-4ded-ae1f-3fa2abd25c56)

Looking into the datasets in Rita:

```
rita view hunting_2
```

![Pasted image 20241008085911](https://github.com/user-attachments/assets/e42e8dad-bbc7-4cdb-b09c-24fbc27aaa77)

> [!NOTE]  
> Rita is pointing out that  "cisco-update.com" has many unique subdomains calls.

This is an abnormally high amount of traffic and something that needs to be investigated further. 

## Tshark Manual Analysis:

Tshark is a command-line network protocol analyzer that is part of the Wireshark suite. It provides powerful filtering and reporting capabilities, making it useful for troubleshooting network issues and monitoring network performance.

![Pasted image 20241008092856](https://github.com/user-attachments/assets/3738c762-3d2c-4d9f-8d16-de870bb4fbf9)

These are the queries that I ran in order to analyze the pcap:

1. Top talkers:

```
tshark -r dns_tunneling.pcap -q -z conv,ip,ip
```

Here I was able to see that the same internal address had the most traffic going to another 2 internal address and one public IP address:

![Pasted image 20241008102758](https://github.com/user-attachments/assets/350ad280-b09e-4871-b3d2-77e5d663842f)

2. DNS Query Statistics:

```
tshark -r dns_tunneling.pcap -Y "dns" -q -z io,stat,0
```
I was able to see the statistics related to DNS in the pcap:

![Pasted image 20241008102456](https://github.com/user-attachments/assets/07df2bb6-4731-4518-b7d4-7b7b44a805ac)

3. Extracting and analyzing dns queries:

```
tshark -r dns_tunneling.pcap -T fields -e dns.qry.name | head
```
Looking at the dns queries, I can also see the same information that was presented by Rita above pointing to the many subdomains:

![Pasted image 20241008091104](https://github.com/user-attachments/assets/47fe52b3-be81-4454-9ab4-e47752ba3b9f)


4. Count DNS Queries by Domain

```
tshark -r dns_tunneling.pcap -Y "dns" -T fields -e dns.qry.name | sort | uniq -c | sort -nr | less
```

To see exactly how many queries were made to each domain from largest to smallest amount:

![Pasted image 20241008092426](https://github.com/user-attachments/assets/7056958d-91e7-464a-9ee1-b1240adce8a4)

5. Count for a single repetitive FQDN:

```
shark -r dns_tunneling.pcap -Y "dns" -T fields -e dns.qry.name | grep cisco-update.com | wc -l
```

I was able to see that there were 300,000+ queries:

![Pasted image 20241008092645](https://github.com/user-attachments/assets/6ac3775b-e857-45d1-a51c-4c629a93d922)

6. Identify long domain names in DNS queries:

```
tshark -r dns_tunneling.pcap -Y "dns.qry.name matches \"^.{30,}\"" -T fields -e dns.qry.name
```

This allowed me to see just how long the subdomains were and how many had consisted of more than 30 characters:

![Pasted image 20241008101339](https://github.com/user-attachments/assets/49f95458-23a5-49d1-89e2-4eb66e7c3b0d)

## Wireshark:

Looking at the I/O Graphs in Wireshark:

![Pasted image 20241008102158](https://github.com/user-attachments/assets/ab4476b6-08e4-4aa9-96c4-aa02db020b74)

## Conclusion:

Based on the information gathered, this looks like a DNS tunneling attack. I came to this conclusion based on the unusual traffic patterns, such as high volumes of DNS requests to unusual domains and the queries with excessively long subdomains, which can be a sign of tunneling activity.

According to the cyber kill chain model, specifically during the "Actions on Objectives" phase, attackers often exfiltrate data using various methods, including DNS tunneling, SSL tunneling, ICMP tunneling, and SSH tunneling. In DNS tunneling, an attacker establishes a server to receive DNS queries and respond to them, while deploying a malicious program on the client to facilitate continuous DNS queries to the compromised server. Tools like Iodine and dnscat can be used to create and manage these DNS tunnels effectively.

MITRE Tactics: T1071.004 DNS, TA0011 Command and Control, T1048 Exfiltration Over Alternative :

![Pasted image 20241008095338](https://github.com/user-attachments/assets/43ac4ba3-0fb4-495d-b296-0bf5763559c7)

### Summary:

In this exercise, I was able to use tools like Tshark, Zeek, and Rita to analyze a PCAP file which revealed critical insights and helped in identifying DNS tunneling activity. This granular visibility into network traffic allowed me to recognize the indicators of compromise associated with DNS tunneling. I was able to learn how to identify malicious activity through anomaly detection, gained insights into exfiltration techniques, and deepened my understanding of DNS. 
