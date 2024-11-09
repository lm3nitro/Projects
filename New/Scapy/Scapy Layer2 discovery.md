## Introduction

I will be utilizing scapy to send ARP requests to my entire network, which enables the discovery of active devices and the creation of IP-MAC address mappings. This process identifies which hosts are currently connected, providing insights into the network's structure and topology.

## Use Cases

+ By sending ARP requests, an attacker can create a detailed map of the network, identifying active devices and their associated IP and MAC addresses. This information can help them understand the network layout and plan further attacks.
+ ARP requests can also be utilized to maintain an up-to-date inventory of devices, aiding in asset management and the detection of unauthorized devices.
+ This can also aid in troubleshooting, as these requests can help identify misconfigured or unreachable devices, facilitating quicker resolution of connectivity issues.

## Getting Started:

This is the script I created and utilized using scapy:

```python

from scapy.all import *  
  
# Define the target subnet  
target_subnet = "10.10.100.0/24"  
  
# Send ARP requests  
answered, unanswered = srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=target_subnet), timeout=2, verbose=False)  
  
# Print out the list of discovered hosts  
for sent, received in answered:  
    print(f"Host Up: {received.psrc} MAC: {received.hwsrc}")
```

This is a view of what it looks like in PyCharm:

![Pasted image 20241009234740](https://github.com/user-attachments/assets/6cfd6085-34c6-4a13-8018-e62a38312e44)

Once executed, these were the results. I was able to see the hosts IP addresses along with their corresponding MAC addresses:

![Pasted image 20241009234458](https://github.com/user-attachments/assets/bc259adf-070d-44e6-914c-36ceaf9770e1)

I also ran Wireshark while the script was running so that I could see the behavior on the network:

![Pasted image 20241009234024](https://github.com/user-attachments/assets/ff3c1dec-89bf-4b42-8afd-757f15ed3d94)

## Summary:

Using Scapy to map devices in the network allowed me to gain hands-on experience in network discovery and monitoring. By crafting and sending ARP requests, I was able to identify all active devices and create a comprehensive map of their IP and MAC addresses. This process also enabled me to detect anomalies associated with Layer 2, such as unexpected MAC addresses or changes in the expected IP-MAC mapping, which could indicate issues like ARP spoofing or unauthorized devices. 
