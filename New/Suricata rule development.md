# Suricata Rule Development

![Pasted image 20241009195326](https://github.com/user-attachments/assets/cbc68079-3124-4b23-bc77-329d3e07b1e5)

Suricata is an open-source network threat detection engine designed for intrusion detection, intrusion prevention, and network security monitoring. It is capable of processing high volumes of traffic in real-time. Here are some key features:

+ Deep packet inspection and packet capture logging
+ Anomaly detection and Network Security Monitoring
+ Intrusion Detection and Prevention, with a hybrid mode available
+ Lua scripting
+ Geographic IP identification (GeoIP)
+ Full IPv4 and IPv6 support
+ IP reputation
+ File extraction
+ Advanced protocol inspection
+ Multitenancy

### Scope:

The scope of this project involves installing Suricata on a VM to serve as a dedicated network intrusion detection and prevention system. This setup allows for easy management and scalability in a controlled environment. After installation, I will create custom rules and will simulate various scans traffic to test the rules effectiveness. I will also monitor and analyze the results through Suricata's alert logs. 

### Tools and Technology:

Ubuntu and Suricata 

## Installation

These are the commands I used to install Suricata, and packages needed:

```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata
```

Once Suricata was installed, I went ahead and upgraded to ensure everything was up to date:

```
sudo apt-get update
sudo apt-get upgrade suricata
```

> [!TIP]
> To remove suricata, use the command below
> ```
> sudo apt-get remove suricata
> ```

Once I updated/upgraded the packages, I then updated Suricata's default set of rules:

```
suricata update
```

![Pasted image 20241008150324](https://github.com/user-attachments/assets/0287d822-8321-4525-8698-ed060c031355)

Next, I needed to configure Suricata. In the screenshot below, I performed the following changes as part of the configuration

+ Enabled pcap logging
+ Disabled suricata.rules and added the local.rule "custom" path
+ Added the network range to the HOME_NET variable:

![Pasted image 20241008163021](https://github.com/user-attachments/assets/0683f298-fe4c-430e-80ce-c3aab989ca21)

> [!TIP]
> In order to process/analyze pcap files, the following syntax can be used:
> ```
> suricata -r /home/lm3nitro/pcaps/lm3nitro.evil.pcap
> ```
> An alternative command can be executed to bypass checksums (-k flag) and log in a different directory (-l flag):
> ```
> suricata -r /home/lm3nitro/pcaps/lm3nitro.evil.pcap -k -l .
> ```
> For live input,you can use Suricata’s (Live) LibPCAP mode as follows:
> ```
> sudo suricata --pcap=ens160 -vv
> ```

## Suricata Rule Development

### 1. Suricata rule to detect NMAP Ping Sweep:

Created rule:

```
alert icmp any any <> any  any (msg:"Potential ICMP Ping Sweep detected"; itype:8; threshold: type both, track by_src, count 5, seconds 10; sid:1000003; rev:1;)
```

To test against the rule created, I performed a ping sweep with using nmap:

```
nmap -sn 192.168.1.0/24
```

![Pasted image 20241008160247](https://github.com/user-attachments/assets/f66ad021-2d7e-4922-ba8e-6c208a7617fc)

Captured the traffic with TCPdump:

![Pasted image 20241008160458](https://github.com/user-attachments/assets/1c2d0b75-9cb7-4637-b85d-23fe77337942)

I then read the pcap using Suricata:

![Pasted image 20241008160350](https://github.com/user-attachments/assets/895e0ae2-9300-482f-89eb-70b9281ec675)

Listing the log files:

![Pasted image 20241008161553](https://github.com/user-attachments/assets/5a10c42e-0ce4-4dca-b5a2-0d7d5978570e)

Suricata fast.log "alert logs":

![Pasted image 20241008160146](https://github.com/user-attachments/assets/b9d13a0c-4301-4617-82bd-fa0359a21257)

based on the output, the rule created worked and it was able to detect the ping sweep performed. 

Taking a look at the pcap file to verify the behavior of the ping sweep:

![Pasted image 20241008162132](https://github.com/user-attachments/assets/a5864486-7054-402f-8b82-c4ff8e522306)

### 2. Detecting TCP SYN Scans:

Created rule: 

```
alert tcp any any -> any any (msg:"Nmap TCP SYN Scan detected"; flags:S; threshold: type both, track by_src, count 10, seconds 1; sid:1000002; rev:1;)
```
```
alert tcp any any -> any any (msg:"Nmap half-open SCAN)"; flow:to_server,stateless; flags:S; window:1024; tcp.mss:1460; threshold:type threshold, track by_src, count 20, seconds 70; sid:10000001; priority:1; rev:1;)
```

I then performed a SYN Scan with nmap:

```
nmap -sS 10.10.100.1
```

The scan detected 2 open ports:

![Pasted image 20241008164019](https://github.com/user-attachments/assets/336f5c55-01af-4afc-a127-f5dc2c6f3f55)

Capured the traffic with TCPdump, 2388 packets were received:

![Pasted image 20241008164212](https://github.com/user-attachments/assets/d6274292-9d14-47de-bd57-8f7721f25697)

Read the pcap with Suricata:

![Pasted image 20241008164502](https://github.com/user-attachments/assets/e55078f2-8a1c-4646-a040-ce0285eefabb)

I then took a look at the Suricata fast.log (alert logs). The output shows the alerts generated by the Nmap scanner:

![Pasted image 20241008164441](https://github.com/user-attachments/assets/c3ad9aa1-68a6-4bcb-a654-200d85380138)

Analyzing the pcap file:

![Pasted image 20241008164938](https://github.com/user-attachments/assets/dbed3347-315b-4292-8daf-9ebff6b843c2)

Screenshot of the custom rules thus far:

> [!NOTE]  
> File path: /home/lm3nitro/Desktop/local.rules

![Pasted image 20241009194605](https://github.com/user-attachments/assets/3c39afeb-fb8b-48a0-a5d6-7f775ed9d019)

### 3. Detecting ACK Scans:

Rule created:

```
alert tcp any any -> any any (msg:"Nmap TCP ACK Scan detected"; flags:A; threshold: type both, track by_src, count 10, seconds 1; sid:1000002; rev:1;)
```

Once I created the rule, I performed an ACK San with Nmap to test against it:

![Pasted image 20241009171651](https://github.com/user-attachments/assets/530501f8-a609-400a-8aae-bb4195671875)

Capuring the traffic with TCPdump:

![Pasted image 20241009171910](https://github.com/user-attachments/assets/2a49ba3d-c88f-4011-8e30-7b3722645e52)

Read the pcap with Suricata:

![Pasted image 20241009171837](https://github.com/user-attachments/assets/06160e4b-0bfc-4f3c-bd10-99f69e76063c)

Suricata fast.log "alert logs":

![Pasted image 20241009171801](https://github.com/user-attachments/assets/364dad6a-226f-4704-ba09-409d2bf240b9)

Analyzing the pcap file:

![Pasted image 20241009172156](https://github.com/user-attachments/assets/94367d83-681a-4377-97f9-644b695bdd4b)

### 4. Detecting RST Scans:

> [!TIP]
> Custom Scan Types with --scanflags:
> The --scanflags option in Nmap allows users to specify custom TCP flags for their scan. This option is particularly useful for crafting specialized TCP packets. By default, Nmap uses a set of standard flags for its various scan types. With --scanflags, you can explicitly define which flags to set in the TCP header. For example, you can choose to send SYN, FIN, ACK, or RST flags, among others,

<!-- The --scanflags argument can be a numerical flag value such as 9 (PSH and FIN), but using symbolic names is easier. Just mash together any combination of URG, ACK, PSH, RST, SYN, and FIN. For example, --scanflags URGACKPSHRSTSYNFIN sets everything, though it's not very useful for scanning. The order these are specified in is irrelevant. In addition to specifying the desired flags, you can specify a TCP scan type (such as -sA or -sF). That base type tells Nmap how to interpret responses. For example, a SYN scan considers no-response indicative of a filtered port, while a FIN scan treats the same as open|filtered. Nmap will behave the same way it does for the base scan type, except that it will use the TCP flags you specify instead. If you don't specify a base type, SYN scan is used. -->

```
alert tcp any any -> any any (msg:"Nmap TCP RST Scan detected"; flags:R; threshold: type both, track by_src, count 5, seconds 1; sid:1000003; rev:1;)
```

Suricata fast.log "alert logs":

![Pasted image 20241009173529](https://github.com/user-attachments/assets/21cd873b-23dd-4ef2-92bb-311edbab7474)

Analyzing the pcap file:

![Pasted image 20241009173856](https://github.com/user-attachments/assets/c0e8946d-3209-4e24-9cfe-84e878408afc)

### 5. Detecting FIN Scans

Below is the rule I created:

```
alert tcp any any -> any 1:1024 (msg:"Nmap TCP FIN Scan detected"; flags:F; threshold: type both, track by_src, count 5, seconds 1; sid:1000002; rev:2;)
```

Suricata fast.log "alert logs":

![Pasted image 20241009174738](https://github.com/user-attachments/assets/8c2695e1-1cd2-485a-8c31-1bc2660693ec)

Analyzing the pcap file:

![Pasted image 20241009175808](https://github.com/user-attachments/assets/33ecbd52-3d99-4aaf-8fb2-8343d225b17a)

Using jq to filter alerts:

jq 'select(.alert )' eve.json | head -20

![Pasted image 20241009194146](https://github.com/user-attachments/assets/3da64699-160d-4104-8601-931c356ee794)

### 6. Detecting Fragmentation Scans:

Rule created:

```
alert ip any any -> any any (msg:"Nmap FRAG Scan detected)"; fragbits:M+D; threshold:type limit, track by_src, count 3, seconds 1210; sid:10000004; rev:1;)
```

Suricata fast.log "alert logs":

![Pasted image 20241009180200](https://github.com/user-attachments/assets/5b25864c-d933-4cca-b0a2-3f5e6aa573eb)

![Pasted image 20241009180116](https://github.com/user-attachments/assets/57149621-1464-4082-8141-d2c49b3791fa)

Analyzing the pcap file:

![Pasted image 20241009175649](https://github.com/user-attachments/assets/9cadef88-76ae-4752-bec8-ccd76fd4b455)

### 7. Detecting Smax Scans:

```
alert tcp any any -> any any (msg:"Nmap SMAX Scan detected)"; flags:FPU; flow:to_server,stateless; threshold:type threshold, track by_src, count 3, seconds 120;  sid:10000001; rev:1;)
```

![Pasted image 20241009181625](https://github.com/user-attachments/assets/c5aa2ed6-2c69-43f7-9368-112f0c454509)

Suricata fast.log "alert logs":

![Pasted image 20241009181045](https://github.com/user-attachments/assets/a09a2e9e-8799-486b-866a-5a558e68b387)

Analyzing the pcap file:

![Pasted image 20241009181125](https://github.com/user-attachments/assets/10448e30-9b47-445e-9473-0e64bdcf0754)

### 8. Detecting -sT Connect Scans

Rule generated:

```
alert tcp any any -> any any (msg:"SYN-ACK 3-WAY SCAN)"; flow:to_server; window:64240; flags:S; threshold:type threshold, track by_src, count 20, seconds 70;  sid:10000001; rev:1;)
```

Suricata fast.log "alert logs":

![Pasted image 20241009182423](https://github.com/user-attachments/assets/a87479f9-897d-49f7-a4df-c512146dda82)

Analyzing the pcap file:

![Pasted image 20241009182644](https://github.com/user-attachments/assets/f6f09cde-bfb0-4080-b73a-743aa02a2489)

### Summary:

In implementing Suricata on a VM, the project aimed to enhance network security by developing and testing custom rules against various scans. Through the simulation of network traffic, I was able to evaluate the effectiveness of these rules created, which led to a deeper understanding of Suricata’s capabilities. 

This exercise highlighted the necessity of continuous rule optimization to effectively detect against threats while minimizing false positives. Testing is a crucial component, as it validates the detection capabilities of the rules, informs of adjustments needed based on actual network behavior, and enhances the overall security posture. 

