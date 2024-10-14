



![Pasted image 20241008105723](https://github.com/user-attachments/assets/1e8a5b49-e1d3-477c-a7dd-e9589339c833)


## Identify weird ICMP traffic:

Responds and replay have different payload.  

ICMP is more than a diagnostic protocol. ICMP is also capable of shuttling data between two systems. Unfortunately, this makes ICMP an appealing vector for attacks that can secretly transport commands and exfiltrate data through ICMP tunneling.


![Pasted image 20240912122001](https://github.com/user-attachments/assets/bf9b27fb-db64-4867-be88-45618bc96421)



C2 channel listing directories, the payload has changed. 

ICMP tunnelling is a command-and-control (C2) attack technique that secretly passes malicious traffic through perimeter defences. Malicious data passing through the tunnel is hidden within normal-looking ICMP echo requests and echo responses.


![Pasted image 20240912122259](https://github.com/user-attachments/assets/3b980dc9-7eea-482c-a03d-fa501b483c16)


### Time to Live:



![Pasted image 20240912123801](https://github.com/user-attachments/assets/ad13484a-6373-4616-afce-f8304740d72d)



![Pasted image 20241008105755](https://github.com/user-attachments/assets/6778fbe1-abc9-4775-ae56-9125bb9bf907)


### Decoding the ICMP payload with Tshark and XXD:

Extractinfg the payload in hex:


tshark -n -r weird-ping.pcap -T fields -e "data.data" -Y data.data


![Pasted image 20240912123437](https://github.com/user-attachments/assets/819d9525-1a8b-4272-b1ea-357350ee3823)

### Converting the HEX:

tshark -n -r weird-ping.pcap -T fields -e "data.data" -Y data.data | xxd -r -p | less


![Pasted image 20240912123315](https://github.com/user-attachments/assets/9eaedd7d-879e-49f5-833a-0c6bca0d620d)


### How to Prevent ICMP Tunneling

Because ICMP helps maintain healthy network connections, blocking all ICMP traffic can create challenges. Known malicious endpoints and domains discovered through threat intelligence can be blocked at the perimeter. You can also configure firewalls to block outbound pings to external endpoints and only allow fixed-sized ICMP packets to pass through firewalls.