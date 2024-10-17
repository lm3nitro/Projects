In this script, I will be utilizing Scapy to craft UDP packets sent to port 1 with a string of integers in each packet. Custom UDP packets can help assess how routers, firewalls, and other network devices handle unusual or malformed packets, particularly with specific payloads. It can also serve as a valuable exercise for understanding how networking protocols work.

Here is a copy of the script:
```

from scapy.all import *  
  
# Define the target IP and port  
target_ip = "127.0.0.1"  # Replace with your target IP  
target_port = 1         # Replace with your target port  
  
# Loop through numbers 1 to 10 and send each as a separate packet  
for p in range(1, 11):  
    payload = str(p)  # Converting the numbers to a string  
    packet = IP(dst=target_ip) / UDP(dport=target_port) / payload  # Creating the packet  
    send(packet)  # Sending the packet loopback address  
    print(f"Sent packet to {target_ip}:{target_port} with payload: '{payload}'")
    
```

This is the script in pycharm:

![Pasted image 20241006141946](https://github.com/user-attachments/assets/e5fc8065-6463-4105-a3aa-9b5de7cac09a)

Once the sxcript was executed, I was able to see the 10 packets that were sent including the incremental payload that each one contained:

![Pasted image 20241006141859](https://github.com/user-attachments/assets/8dc7bd99-fb98-4e9d-a1a6-469d80e4be38)

This is a view at the network traffic in Wireshark to visualize the behavior:

![Pasted image 20241006141550](https://github.com/user-attachments/assets/71b5968e-2f09-45ab-888c-470fe3897e25)

Here the malformed packets are seen. We can also see the details of the decoded traffic and the combined string of all the packets together:

![Pasted image 20241006142142](https://github.com/user-attachments/assets/7395ac52-f7c6-419d-bacd-c62811cf39d9)

## Packet Analysis (payload reconstruction)

We can also decode using python. In order to do this, i first located the location of the pcap that was generated:

![Pasted image 20241006144408](https://github.com/user-attachments/assets/1894d96d-09ae-4b5e-98aa-2c46ae401fa0)

This is the script that I will be using:

```
from scapy.all import *  
pcap_file = "/home/lm3nitro/Desktop/string_from_1_to_10.pcapng"  
print(f"[+] Reading pcap from {pcap_file=}....")  
packets = rdpcap(pcap_file)  
  
for p in packets:  
    print(p[Raw].load.decode("utf-8"), end="")
```

Running the script in Pycharm:

![Pasted image 20241006222438](https://github.com/user-attachments/assets/39177fff-8a51-418e-a027-cce91a95115a)

The script decoded the pcap file successfully.

