## Introduction

I will be utilizing python along with the scapy library to create a script that is meant to test and verify what ports are being allowed on my Fortinet Firewall. Identifying open ports helps in evaluating the security posture of a system or network. Open ports can be potential entry points for attackers, so knowing which ones are accessible allows for better risk management and prioritization of security measures.

<img width="370" alt="Screenshot 2024-10-19 at 10 25 39 PM" src="https://github.com/user-attachments/assets/7e1434c5-79f4-45e4-a8ff-4ec6ddb066d6">

## Use Cases:

+ During vulnerability assessments, scripts can be used to map out open ports, helping to identify potentially vulnerable services running on those ports.
+ Penetration tests can use port scanning scripts to gather intelligence on open ports and services, forming the basis for further exploitation attempts or security recommendations.
+ Automated scripts can also be scheduled to run at regular intervals, continuously monitoring open ports to identify if any unauthorized changes are made.

## Script

```python
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

> [!NOTE]
> 
> TCP Flags:
> 
> URG (Urgent): Indicates that the urgent pointer field is significant.
> Hex: 0x20
> 
> ACK (Acknowledgment): Indicates that the acknowledgment field is significant.
> Hex: 0x10
>
> PSH (Push): Indicates that the receiver should pass the data to the application immediately.
> Hex: 0x08
>
> RST (Reset): Indicates that the connection should be reset.
>Hex: 0x04
>
> SYN (Synchronize): Used to initiate a connection.
> Hex: 0x02
>
> FIN (Finish): Indicates that the sender has finished sending data.
> Hex: 0x01

Another view of the script in Pycharm:

![Pasted image 20241006162717](https://github.com/user-attachments/assets/16a6a7c0-4b0a-4a8f-b7ba-7cf87c8dab37)

Executing the script:

![Pasted image 20241006161334](https://github.com/user-attachments/assets/13db4237-c0f1-4987-8cbc-7c155608ce1f)

Port 80 is open!

Sniffing traffic with TCPDump. Here we can see that the firewall only responded to the packet sent to port 80:

![Pasted image 20241006161411](https://github.com/user-attachments/assets/aadc0ab8-2433-478f-bce5-efa42cb2b7ae)

## Summary: 

By running this script against my firewall, I was able to correctly identify that port 80 is open. Utilizing scripts to test open firewall ports is a crucial activity for enhancing security, verifying configurations, troubleshooting issues, ensuring compliance, and supporting proactive network management.
