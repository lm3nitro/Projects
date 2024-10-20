## Introduction

With the following script, I will be sending a TCP packet that contains a custom payload with the Push flag set. The Push flag prompts the receiving TCP stack to deliver the data to the application layer without waiting for additional packets. The primary objective is to test the rules on my Fortinet firewall and evaluate the its responsiveness. 

## Use Cases

+ Sending a variety of bad payloads to check how the firewall logs these incidents and whether alerts are triggered appropriately.
+ Crafting payloads that deviate from standard protocol behavior  to test if the firewall can detect anomalies.
+ Sending application-layer payloads, such as those targeting web applications (e.g., SQL injection attempts), to test the firewall’s application layer filtering capabilities.
+ Crafting and sending payloads designed to match specific firewall rules (both legitimate and malicious) to evaluate whether the firewall correctly applies those rules.

## Script

```
from scapy.all import *  
  
target_ip = "10.10.100.1"  
custom_payload = "lm3nitro_bad_payload"  
  
# Create a custom TCP packet  
packet = IP(dst=target_ip) / TCP(dport=80, flags='P') / Raw(load=custom_payload)  
  
# Send the packet  
send(packet, verbose=False)  
  
print("Sent custom packet to test firewall response.")
```

Another view of the script in Pycharm:

![Pasted image 20241006152913](https://github.com/user-attachments/assets/c8aed9b8-91fb-4543-91bc-013fc9b30d06)

Executing the script:

![Pasted image 20241006153738](https://github.com/user-attachments/assets/0eb834a9-e7ee-4ed2-b9a0-7526955ce8e2)

Sniffing the packet with TCPDump:

![Pasted image 20241006154302](https://github.com/user-attachments/assets/d3929b8b-ebff-47cd-b5b2-85b37895235f)

## Summary:

I was able to send the TCP payload with the custom payload. While the packet was being sent, I was also able to capture the traffic using TCPdump and see the custom payload that was being sent. Performing this exercise allowed me to see my firewalls response when recieving these types of packets. Of course, the payload that was sent in this case was not malicious, however, this type of test can be done to evaluate the ability to detect anomalies, and examine the effectiveness of logging and alerting mechanisms.  

