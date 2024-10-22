## Scenario:

A target host restricts access by only accepting traffic from specific source and destination ports, enhancing security by allowing only authorized services. This minimizes exposure to external threats, ensuring that only designated IPs and ports can interact with the server, thereby reducing the attack surface and preventing unauthorized access or probing.

## Script:

To determine which specific source and destination ports the target host accepts, the script can systematically test various combinations by sending TCP packets with different source ports to a range of destination ports. By executing this process iteratively, the script can effectively map out which source and destination ports the target host is configured to accept, helping to understand its security posture and communication rules.

## Use Cases:

+ An attacker might use it to identify open ports and accepted source ports on a target system, which can help in mapping the network and finding potential vulnerabilities to exploit. If an attacker discovers that certain source and destination ports are open, they could craft packets that mimic legitimate traffic to bypass firewalls or intrusion detection systems, allowing unauthorized access to sensitive services.
+ This also allows for monitoring how the firewall responds to legitimate traffic while blocking threats, and evaluating the IDP/IPS’s ability to detect and respond to intrusions. The results can then be used to fine-tune the configurations, ensuring robust security measures are in place.


## Server Configuration:

![Pasted image 20241013191430](https://github.com/user-attachments/assets/10e07911-89e8-4505-82c6-50d81441725f)

In the exercise below, these are the IPs that will be used:

+ Client: 10.10.100.39
+ Server: 10.10.100.48

### Allow incoming traffic on a specific source port:

In order to test, I configured the server using iptables to only accept traffic from a given IP, source port, and destination port. 

```
sudo iptables -A INPUT -p tcp -s 10.10.100.39 --sport 10 -d 10.10.100.48  --dport  22 -j ACCEPT


sudo iptables -A INPUT -p tcp -s 10.10.100.39 --sport 5 -d 10.10.100.48  --dport  22 -j ACCEPT

sudo iptables -A INPUT -p tcp -s 10.10.100.39 --sport 2 -d 10.10.100.48  --dport  22 -j ACCEPT

```

I also configured the server to deny any other incoming traffic outside of the previosuly slected IP, source port and destination port combination:


```
sudo iptables -A INPUT -p tcp -s 10.10.100.39 -d 10.10.100.48 --dport  22 -j DROP
```


![Pasted image 20241013182339](https://github.com/user-attachments/assets/6afd356b-abd6-4e4b-9574-65b86aab0f32)


### Scanning the server with Nmap:

To ensure that everything was configured as expected, I used namp:

Based on the output below, the port has been filtered by the IPtable rules:

![Pasted image 20241013182741](https://github.com/user-attachments/assets/36997f92-3948-4815-a8bb-23ccd67b9b7b)

### Network trafifc from the client and server:

The server doesn't response to any probes from the client.

![Pasted image 20241013183416](https://github.com/user-attachments/assets/92769562-5377-42ef-b724-79108f95634c)


## Script creation:

As previousky stated in the beginning, I will be utilizing Python and along with the Scapy library:

![Pasted image 20241013191549](https://github.com/user-attachments/assets/0288b8c6-e0cc-426f-8204-29b82dd6a44e)

This is the script I created and used to canning the server with specify source ports:

```python

from scapy.all import *  
  
  
def test_ports(target_ip):  
    for source_port in range(1,11):  # Start from source port 20 to 22
        for dest_port in range(20, 23):  # Start from destination port 3127 to 3128  
            # Creating a SYN packet with the current source and destination ports            syn_packet = IP(dst=target_ip) / TCP(sport=source_port, dport=dest_port, flags="S")  
  
            # Sending the packet and wait for a server response  
            response = sr1(syn_packet, timeout=1, verbose=0)  
  
            # Checking the server response  
            if response is None:  
                print(f"[{source_port}] Port {dest_port} is filtered (no response)")  
            elif response.haslayer(TCP):  
                if response.getlayer(TCP).flags == 0x12:  # SYN-ACK  
                    print(f"[{source_port}] Port {dest_port} is open")  
                    # Sending a RST packet to close the connection with the server  
                    rst_packet = IP(dst=target_ip) / TCP(sport=source_port, dport=dest_port, flags="R")  
                    send(rst_packet, verbose=0)  
                elif response.getlayer(TCP).flags == 0x14:  # RST-ACK flag  
                    print(f"[{source_port}] Port {dest_port} is closed")  
  
  
  
if __name__ == "__main__":  
    target_ip = input("Provide server IP: ")  
    test_ports(target_ip)
```


### Pycharm IDE:

![Pasted image 20241013202456](https://github.com/user-attachments/assets/cabcdfd5-e170-4935-ac9c-0d3dcb51c02d)


### Running the script:

Scanning the server from source 1-10 and destination 20-22, the scanner was able to find open ports coming on source port 2, 5 and 10.

![Pasted image 20241013182212](https://github.com/user-attachments/assets/7b62164f-bc17-47d6-b261-e3e3f5e2f9a7)

### Network traffic from the Scapy scanner:

![Pasted image 20241013183308](https://github.com/user-attachments/assets/4afbcd18-0c88-4cf2-b814-e46e5dd70142)

### Filtering only the SYN-ACK flags from the server:

```
tcpdump -nr only_src_ports_allow.pcapng 'tcp[tcpflags] & (tcp-syn|tcp-ack) == (tcp-syn|tcp-ack)'
```
![Pasted image 20241013183145](https://github.com/user-attachments/assets/51142b2e-fe2a-4bc1-9850-17f0132792a7)

The traffic is only allowed if the source port is 2,5 or 10:

## Connecting to SSH Binding to a specific source port:

Since the server is allowing traffic on specific source port. I must proxy the traffic to another utility that allows me to bind the source port.

```
sudo ssh -o 'ProxyCommand nc -p 5 %h %p' elk@10.10.100.48
```

Connecting to the server using a specific source port with netcat, in this case I picked source port 5.

![Pasted image 20241013195546](https://github.com/user-attachments/assets/3df12fa1-ad09-4a3d-a7ea-6e8a1c44dc6c)

Connection state on the server:

![Pasted image 20241013195732](https://github.com/user-attachments/assets/89ad1474-f16d-4026-a881-d6eb7cc35142)

### Network traffic:

![Pasted image 20241013195649](https://github.com/user-attachments/assets/a0aca375-1a84-405e-8049-cff6d89b4860)

## Summary:

This exercise allowed me to understand the importance of access control measures in minimizing exposure to threats and how they can effectively protect sensitive services. I also gained insights into how limiting communication to authorized IPs and ports reduces the attack surface, making it harder for unauthorized entities to exploit vulnerabilities. This also made me better appreciate the need for continuous monitoring and adjustment of firewall rules and security policies to adapt to evolving threats.
