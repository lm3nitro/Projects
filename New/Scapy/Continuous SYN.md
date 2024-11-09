## Introduction:

In this script below, I will be using the scapy library to send custom TCP packets to port 22 with a SYN flag set in a continuous loop. The primary objective is to see how the target host responds to this type of traffic being sent. For the purpose of this exercise, my target will be my Fortinet firewall. 

## Use Cases:

+ SYN flood scripts can be used to simulate an attack, helping to identify vulnerabilities in network infrastructure.
+ By generating SYN packets, you can evaluate the effectiveness of an IDS in detecting and responding to unusual traffic patterns.
+ Sending SYN packets can help assess how a network responds to incoming connection requests.


```python
from scapy.all import *  
  
target_ip = "10.10.100.1"  # Replace with the target IP  
target_port = 22  # Port to target  
  
# Generate a SYN packet and send it in a loop  
while True:  
    syn_packet = IP(dst=target_ip) / TCP(dport=target_port, flags='S')  
    send(syn_packet, verbose=False)
```

Code in PyCharm:

![Pasted image 20241006155328](https://github.com/user-attachments/assets/ecbc51c2-bf5b-417f-b98a-0c79320e7d12)

Executing the script and sniffing the packet with Tcpdump:

![Pasted image 20241006155917](https://github.com/user-attachments/assets/48ce550e-f633-43fb-bff4-c176ccc8980c)

Here I was able to see that the firewall is responding on port 22:

![Pasted image 20241006160651](https://github.com/user-attachments/assets/f9579558-713c-4e83-845c-8e5ef8e3c085)


## Summary:

After executing the script, I was able to see the traffic in real time. I could also see that every time the firewall received a SYN packet it responded with a RST packet. A firewall will usually respond in this manner for several reasons:

+ The destination port is closed or not open for connections
+ The traffic does not meet the firewall's security policies or rules
+ The incoming traffic is from an untrusted source

In this case, the traffic was coming from a trusted source, but the port is closed and the firewall responded in the expected manner. Overall, it is important to run tests against firewall rules to ensure that it effectively blocks unauthorized access while allowing legitimate traffic. Regular testing helps maintain the integrity, performance, and security of the network.

