# Argus

Argus, created by Carter Bullard in the 1980s is a network audit technology. It analyzes all network traffic, supporting both small and large-scale audits. Argus provides real-time data for immediate action or storage, available on various Linux systems.

In my lab, I decided to install it in Ubuntu:
```
sudo apt install argus-server  
sudo apt install argus-clients
```
![Pasted image 20240402150020](https://github.com/lm3nitro/Projects/assets/55665256/77382312-bc0f-4206-a883-967d9db44de0)

### Getting help:
```
argus -h 
```
![Pasted image 20240402150554](https://github.com/lm3nitro/Projects/assets/55665256/c38447ee-365f-42dc-866b-cc011e03d9e5)

### Flow analysis with Argus:

![Pasted image 20240402155216](https://github.com/lm3nitro/Projects/assets/55665256/160f952f-5798-42e5-892e-8ec5064f336b)

![Pasted image 20240402160616](https://github.com/lm3nitro/Projects/assets/55665256/53649c0f-2b1e-4080-8505-0730f9205f96)

### Here are a few more examples of how this tool can be used:

1. You can run argus as a daemon, writing all its transaction status reports to output-file. This is the typical mode.
```
argus -d -e `hostname` -w output-file
```

2. If ICMP traffic is not of interest to you, you can filter out ICMP packets on input.
```
argus -w output-file - ip and not icmp
```

3. If you want to audit each individual ICMP ECHO transaction from data in <dir>. You would do this to gather Round Trip Time (RTT) data within your network. You can also append the output to output-file.
```
argus -R dir -w output-file "echo" - icmp
```
4. To import flow data from pcap file containing Cisco flow data packets. Write output to stdout, to a ra.1 instance.
```
argus -r paloalto:pcap-file -w - | ra 
```
I can definitely see how the data that I am able to get from Argus is crucial for network forensics, anomaly detection, and performance monitoring. 
