### Sending SYN  packet in continually :



**Objective**: Craft and send custom packets continuously  on port 22 SSH to test firewall rules.

```

from scapy.all import *  
  
target_ip = "10.10.100.1"  # Replace with the target IP  
target_port = 22  # Port to target  
  
# Generate a SYN packet and send it in a loop  
while True:  
    syn_packet = IP(dst=target_ip) / TCP(dport=target_port, flags='S')  
    send(syn_packet, verbose=False)
```

Code in Pycharm:

![Pasted image 20241006155328](https://github.com/user-attachments/assets/ecbc51c2-bf5b-417f-b98a-0c79320e7d12)


Executing the script and sniffing the packet with TCPDump:


![Pasted image 20241006155917](https://github.com/user-attachments/assets/48ce550e-f633-43fb-bff4-c176ccc8980c)


Firewall is responding on port 22:


![Pasted image 20241006160651](https://github.com/user-attachments/assets/f9579558-713c-4e83-845c-8e5ef8e3c085)

Since the script was going to loop for ever I broke the loop by Ctrl+c. 
