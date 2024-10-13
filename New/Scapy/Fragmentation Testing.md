 The goal is to ensure that the firewall does not allow fragmented malicious traffic to bypass security controls.

**Objective**: Check if the firewall properly handles fragmented packets.


```
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

![Pasted image 20241006192639](https://github.com/user-attachments/assets/2f10aac0-9457-4ba2-8742-ea508e40c33e)


Executing the script and sniffing the traffic with TCPDump to  esting Firewall Resilience Against Fragmentation Attacks


![Pasted image 20241006194801](https://github.com/user-attachments/assets/517e9879-2e66-483a-b6c9-b1ace5ec534d)


 Close look in Wireshark:


![Pasted image 20241006193329](https://github.com/user-attachments/assets/14f55dd9-ccf9-40a5-b9ff-639d793b97c9)

Firewall doesn't response to packets with the flag set.
