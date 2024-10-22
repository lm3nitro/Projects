## Introduction:

I will be using the script I created below to test the behavior of my Fortinet firewall to see the response against fragmented packets. Fragmented packets can be exploited by attackers to bypass firewall rules. Some firewalls might not properly reassemble fragmented packets, allowing malicious payloads to slip through undetected. By testing how a firewall handles fragmentation, you can identify potential vulnerabilities that could be exploited.

<img width="508" alt="Screenshot 2024-10-19 at 10 15 36 PM" src="https://github.com/user-attachments/assets/7a369efc-d246-4650-95bd-ac0ff6ce0dc6">

## Use Cases:

+ By intentionally sending fragmented packets that mimic potential attack vectors, you can assess whether the firewall can detect and respond appropriately.
+ Insights gained from these tests can inform incident response strategies. Knowing this allows you to better prepare for real-world incidents involving fragmentation tactics.
+ Organizations can also perform these tests to ensure compliance. Testing fragmented packet handling can provide evidence that the firewall meets necessary security standards.

## Script

Below is the python script I created using scapy:

```python
from scapy.all import *

target_ip = "10.10.100.1"

# Create a fragmented packet
packet = IP(dst=target_ip, frag=1) / TCP(dport=80, flags='S') / Raw(load="lm3nitro_data_frag")
fragmented_packets = fragment(packet)

# Send fragmented packets
for frag in fragmented_packets:
    send(frag, verbose=False)

print("Sent fragmented packets to test firewall handling.")

```

Here is another view of the script in Pycharm:

![Pasted image 20241006192639](https://github.com/user-attachments/assets/2f10aac0-9457-4ba2-8742-ea508e40c33e)

I executed the script and sniffed the traffic with TCPDump in order to capture it in real time:

![Pasted image 20241006194801](https://github.com/user-attachments/assets/517e9879-2e66-483a-b6c9-b1ace5ec534d)

I then opened the pcap in Wireshark to take a closer look:

![Pasted image 20241006193329](https://github.com/user-attachments/assets/14f55dd9-ccf9-40a5-b9ff-639d793b97c9)

In Wireshark I took a closer look at the packets sent and verified that the fragment flag had been set. I was also able to see that the Firewall did not respond to the packets with this fragmented flag set. 

## Summary:

Using the script above, I was able to test my firewalls behavior when receiving fragmented packets. The way a firewall responds provides valuable insights into potential vulnerabilities, revealing whether or not it can be exploited by attackers using fragmentation techniques to bypass security measures.  
