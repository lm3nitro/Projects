
**Objective**: Verify if the firewall blocks specific traffic as intended.



Code:

```

from scapy.all import *

target_ip = "10.10.100.1"
test_ports = [12, 45, 155, 50, 80]  # Ports to test

# Sending a SYN packet to test if the firewall allows the traffic:
for port in test_ports:
    syn_packet = IP(dst=target_ip) / TCP(dport=port, flags='S')
    response = sr1(syn_packet, timeout=1, verbose=False)

    if response:
        if response.haslayer(TCP):
            if response[TCP].flags == 0x12:  # SYN-ACK
                print(f"Port {port} is open (allowed by firewall)")
            elif response[TCP].flags == 0x14:  # RST-ACK
                print(f"Port {port} is closed (blocked by firewall)")
    else:
        print(f"No response from port {port} (potentially blocked)")

```



## TCP Flags:

URG (Urgent): Indicates that the urgent pointer field is significant.
Hex: 0x20

ACK (Acknowledgment): Indicates that the acknowledgment field is significant.
Hex: 0x10

PSH (Push): Indicates that the receiver should pass the data to the application immediately.
Hex: 0x08

RST (Reset): Indicates that the connection should be reset.
Hex: 0x04

SYN (Synchronize): Used to initiate a connection.
Hex: 0x02

FIN (Finish): Indicates that the sender has finished sending data.
Hex: 0x01


Pycharm IDE:


![Pasted image 20241006162717](https://github.com/user-attachments/assets/16a6a7c0-4b0a-4a8f-b7ba-7cf87c8dab37)

Executing the script:

![Pasted image 20241006161334](https://github.com/user-attachments/assets/13db4237-c0f1-4987-8cbc-7c155608ce1f)

Port 80 is open!

Sniffing traffic with TCPDump:


![Pasted image 20241006161411](https://github.com/user-attachments/assets/aadc0ab8-2433-478f-bce5-efa42cb2b7ae)
