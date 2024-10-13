# Argus

![Screenshot 2024-09-21 at 11 11 25 PM](https://github.com/user-attachments/assets/60f8ce3e-3889-4477-a71c-4d69f28b0161)

Argus, created by Carter Bullard in the 1980s is a network audit technology. It analyzes all network traffic, supporting both small and large-scale audits. Argus provides real-time data for immediate action or storage, available on various Linux systems. It focuses on network flow data and provides in-depth traffic details, offering insights into communications across the network. Argus is commonly used for security analysis, network performance monitoring, and incident response.

### Scope:
I emulated an amplication syn flood attack on one of the nodes in my lab environment. While performing this attack, I generated a pcap. In this project, I will be installing Argus on an Ubuntu VM and using it to analyze the pcap that was captured during the amplication syn flood attack. 

### Tools and Technology:
Ubuntu and Argus

## Installation 

I ran the following commands to install Argus:

```
sudo apt install argus-server  
sudo apt install argus-clients
```

![Pasted image 20240402150020](https://github.com/lm3nitro/Projects/assets/55665256/77382312-bc0f-4206-a883-967d9db44de0)

By using the following command I can also see all the options that Argus offers:

```
argus -h 
```

![Pasted image 20240402150554](https://github.com/lm3nitro/Projects/assets/55665256/c38447ee-365f-42dc-866b-cc011e03d9e5)

## Flow analysis with Argus:

These are the current pcaps that I have generated. I will be taking a closer look into the pcap associated with the emulated amplication syn flood attack. 

![Pasted image 20240402155216](https://github.com/lm3nitro/Projects/assets/55665256/160f952f-5798-42e5-892e-8ec5064f336b)

Using Argus, I ran the following command

```
ra -r tcmp.amp.syn.argus -n -A -N 30
```
For reference, this is a breakdown into the options that I used:

+ -r: Used to choose the packetfile
+ -n: Disable DNS lookup when processing network flow data
+ -A: Provides application byte metrics
+ -N: Specifies how many different service names should be shown (in this case 30)
  
![Pasted image 20240402160616](https://github.com/lm3nitro/Projects/assets/55665256/53649c0f-2b1e-4080-8505-0730f9205f96)

A breakdown from the output Argus generated:
+ Based on the start time, I could see that these packets were being sent within milliseconds of one another
+ The destination address changes with every packet that was sent and only used port 443 and 80
+ The destination IP was the same, but every packet was directed a different destination port
+ The size remained consistant with each being 58 bytes in size

The characteristics described above are that of a typical amplification SYN flood attack. In this case, the attacker generated a large number of SYN packets, leveraging a network of compromised devices (botnet) to amplify the volume of the attack. 

## Additional Argus Features

1. You can run argus as a daemon, writing all its transaction status reports to output-file. This is the typical mode:

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

### Summary:

I really liked using Argus and exploring the different options it has to better analyze network traffic. In this project, I was able to idenitfy and understand the attack characteristics of the amplification SYN flood attack. 

Lessons Learned:

+ Understanding Attack Patterns: Analyzing the pcap file provided practical experience in recognizing SYN flood patterns and amplification techniques, essential for early detection in future scenarios.
+ Importance of Flow Monitoring: The flow analysis offered a high-level view of network behavior, allowing efficient analysis without delving into individual packets, which is particularly useful during large-scale attacks.
+ Need for Continuous Monitoring: The analysis reinforces the necessity of continuous network monitoring to detect anomalies and respond quickly to potential threats, enhancing incident response preparedness.
+ Value of Post-Incident Analysis: Understanding the attack's characteristics and impact prepares teams for better incident response and resource allocation for mitigating future attacks.

Using Argus to analyze this amplification SYN flood pcap offered valuable insights into the nature of this type of atack and the impact on network resources. This is crucial for developing effective detection, mitigation strategies, and enhancing overall network security posture.
