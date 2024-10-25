# DNS Tunneling

DNS tunneling is a difficult-to-detect attack that routes DNS requests to the attacker's server, providing attackers a covert command and control channel, and data exfiltration path. DNS tunneling is a technique that encapsulates data within DNS queries and responses, allowing data to be sent and received through DNS traffic. This method can be used for both legitimate purposes, such as bypassing network restrictions, and malicious activities, such as exfiltrating data or establishing command-and-control channels for malware.

### Scope:

While monitoring network traffic in Kibana, I noticed an unusual spike in DNS queries from a single internal IP address, which significantly exceeded the normal baseline. This anomaly raised concerns about potential DNS tunneling or unauthorized data exfiltration. To investigate further, I drilled down into the logs and network traffic, examining the specific DNS queries being made, the domains being accessed, and the timestamps of the requests. I also cross-referenced this data with user activity logs to identify any recent changes or suspicious behavior associated with that IP address, aiming to determine whether this spike was a benign issue, such as a misconfigured application, or a sign of a more serious security threat.

Topology:
![Pasted image 20241008094034](https://github.com/user-attachments/assets/66dbf3d6-4dad-4bb3-9a34-5987dc8d3b12)


### Tools and Technology:



### Dashboard in Kibana shows DNS spikes:



![Pasted image 20240416141836](https://github.com/user-attachments/assets/7708a48e-4dd2-4624-bfef-d490d49bc7a7)



### Detecting  a DNS tunnelling attack:

Long Domain Names: 
DNS queries with unusually long lengths with more than 30 characters.

Frequent Queries: 
High frequency of requests to the same domain over a short period.

Non-Standard Characters: 
Domains that include special characters or seem random.


### Looking at the pcap information with capinfos:

![Pasted image 20241008083359](https://github.com/user-attachments/assets/35e21ed1-5491-43e4-8b0e-fc871d6f4c0e)




zeek readpcap dns_tunneling.pcap

![Pasted image 20241008083742](https://github.com/user-attachments/assets/77942bb2-a075-45b8-86d8-27d2bb296462)

There are two file with the highes amount of data:

![Pasted image 20241008093150](https://github.com/user-attachments/assets/c9c83bf3-ed9e-4de9-9378-a553a69be8af)



### Importing the zeek  logs to Rita:


![Pasted image 20241008092949](https://github.com/user-attachments/assets/0abb7e95-37c0-4fda-a523-57c4ae6ee3d9)


rita import -l /opt/zeek/manual-logs/ -d hunting_2

![Pasted image 20241008084930](https://github.com/user-attachments/assets/a587c7c2-8124-4774-b41a-98b80b575427)


rita list

![Pasted image 20241008085120](https://github.com/user-attachments/assets/ae22dcce-0fdd-4ded-ae1f-3fa2abd25c56)



###  View the database in Rita:

rita view hunting_2

![Pasted image 20241008085911](https://github.com/user-attachments/assets/e42e8dad-bbc7-4cdb-b09c-24fbc27aaa77)


Note: Rita is pointing out that  "cisco-update.com" has many unique subdomains calls.


### Manual analysis with Tshark:


![Pasted image 20241008092856](https://github.com/user-attachments/assets/3738c762-3d2c-4d9f-8d16-de870bb4fbf9)




### Top talkers

```
tshark -r dns_tunneling.pcap -q -z conv,ip,ip

```
![Pasted image 20241008102758](https://github.com/user-attachments/assets/350ad280-b09e-4871-b3d2-77e5d663842f)

### DNS Query Statistics:

```
tshark -r dns_tunneling.pcap -Y "dns" -q -z io,stat,0
```

![Pasted image 20241008102456](https://github.com/user-attachments/assets/07df2bb6-4731-4518-b7d4-7b7b44a805ac)



### Extract and Analyze Payload:

```
tshark -r dns_tunneling.pcap -T fields -e dns.qry.name | head
```

![Pasted image 20241008091104](https://github.com/user-attachments/assets/47fe52b3-be81-4454-9ab4-e47752ba3b9f)


### Count DNS Queries by Domain

```
tshark -r dns_tunneling.pcap -Y "dns" -T fields -e dns.qry.name | sort | uniq -c | sort -nr | less
```


![Pasted image 20241008092426](https://github.com/user-attachments/assets/7056958d-91e7-464a-9ee1-b1240adce8a4)


### Count for a single repetitive FQDN:

```
shark -r dns_tunneling.pcap -Y "dns" -T fields -e dns.qry.name | grep cisco-update.com | wc -l
```

![Pasted image 20241008092645](https://github.com/user-attachments/assets/6ac3775b-e857-45d1-a51c-4c629a93d922)



### Identify long domain names in DNS queries:

```
tshark -r dns_tunneling.pcap -Y "dns.qry.name matches \"^.{30,}\"" -T fields -e dns.qry.name

```


![Pasted image 20241008101339](https://github.com/user-attachments/assets/49f95458-23a5-49d1-89e2-4eb66e7c3b0d)


### Looking at the I/O Graphs in Wireshark:

Analysing the DNS queries lengths:

![Pasted image 20241008102158](https://github.com/user-attachments/assets/ab4476b6-08e4-4aa9-96c4-aa02db020b74)


### MITRE Tactics: T1071.004 DNS, TA0011 Command and Control, T1048 Exfiltration Over Alternative :

![Pasted image 20241008095338](https://github.com/user-attachments/assets/43ac4ba3-0fb4-495d-b296-0bf5763559c7)


According to cyber kill chain, actions on objective step of the cyber attacks, attackers exfiltrate data with various ways like DNS tunnel, SSL Tunnel, ICMP Tunnel, SSH tunnel. In DNS tunnel Method attacker sets up a server for getting DNS queries and responding it and puts a malicious program to the client for continuous DNS queries to the malicious server. Iodine or dnscat can be used for generating dnstunnel. 

