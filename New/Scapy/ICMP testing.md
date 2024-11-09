## Introduction:

This script utilizes Scapy to send ICMP Requests to a specified host, in this case, my firewall. The primary purpose of this script is to assess the availability and responsiveness of the device by analyzing the received ICMP Reply packets. This is particularly useful for network diagnostics, performance assessment, and security evaluations.

<img width="381" alt="Screenshot 2024-10-19 at 10 17 33 PM" src="https://github.com/user-attachments/assets/56849d0f-e61b-4ffc-b9a2-18791b5d6fb7">

## Use Cases

+ ICMP requests can be used to continuously monitor the availability of critical servers and devices, ensuring they are operational and responding as expected.
+ Adversaries can utilize ICMP requests to discover active devices on a network, creating a map of targets. This information is crucial for planning further attacks, such as identifying high-value systems to compromise.
+ ICMP requests can also be effectively used to diagnose issues related to network connectivity and determine whether ICMP traffic is being blocked
  
## Getting Started

This is the script I created and will be utilizing to send ICMP requests to my firewall on 10.10.100.1:

```python

from scapy.all import *

target_ip = "10.10.100.1"

# Send an ICMP Echo Request (Ping)
icmp_packet = IP(dst=target_ip) / ICMP()
response = sr1(icmp_packet, timeout=1, verbose=False)

if response:
    if response.haslayer(ICMP):
        if response[ICMP].type == 0:  # Echo Reply
            print("Ping successful: Target is reachable.")
        else:
            print("Ping failed: Target is unreachable.")
else:
    print("No response: Target may be blocked.")

```

After executing the script, we can see that it was successful, and it is reachable:

![Pasted image 20241006194551](https://github.com/user-attachments/assets/6c41b891-9251-47bc-9b50-7e34aa394a96)

Here is a view in Wireshark:

![Pasted image 20241006194331](https://github.com/user-attachments/assets/6228c538-5e75-4c2f-b08d-9b10be694d65)

## Summary:

I was able to utilize scapy and python to send ICMP packets targeting my Firewall and analyze the response. I was also able to learn about the firewall's behavior and configurations, which helps assess its effectiveness and identify potential misconfigurations. This script can also be used to effectively troubleshoot connectivity issues and identify whether devices are reachable or not. Overall, creating custom network tools and scripts, makes routine tasks more efficient and enables you to adapt your approach to specific networking needs. 
