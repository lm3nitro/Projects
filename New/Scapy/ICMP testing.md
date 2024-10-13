

**Objective**: Testing how the firewall handles ICMP packets, such as ping requests.

```

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


![Pasted image 20241006194551](https://github.com/user-attachments/assets/6c41b891-9251-47bc-9b50-7e34aa394a96)


![Pasted image 20241006194331](https://github.com/user-attachments/assets/6228c538-5e75-4c2f-b08d-9b10be694d65)
