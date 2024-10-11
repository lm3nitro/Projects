# TCP Hijacking

<img width="752" alt="Screenshot 2024-10-10 at 9 43 48 PM" src="https://github.com/user-attachments/assets/114fd610-0ece-49fd-9a06-c3644866a200">

TCP hijacking involves an attacker intercepting and manipulating TCP sessions to gain unauthorized access to sensitive information. The attacker exploits vulnerabilities such as predictable sequence numbers or weak authentication to impersonate legitimate parties and control the hijacked sessions, allowing for data theft or manipulation. 
To maintain the hijacked session, the attacker must prevent ACKs from reaching the compromised machine, achieved by either delaying or blocking the ACK packets. This tactic is often used alongside ARP poisoning, leading to observable patterns in our traffic analysis.

### How it works: 

1. When two devices communicate over TCP, they establish a connection through a three-way handshake (SYN, SYN-ACK, ACK). After the handshake, they exchange data packets.
2. The attacker can use various techniques to intercept packets being sent between the two devices (Man-in-the-Middle, IP Spoofing, etc)
3. Once the attacker has captured the session's packets, they can take control by:
    + Sending Packets: The attacker sends forged packets that the target device believes are coming from the legitimate source.
    + Resetting the Connection: The attacker may send a packet with a TCP reset (RST) flag, which can terminate the session for one party and allow the attacker to establish a new session.
4. Exploitation: With control over the TCP session, the attacker can perform various malicious actions, such as data manipulation, session takeover, or injection of malicious commands.

### Analysis:
When looking at the pcap, we can identify a large number of ARP requests coming from one particular node when monitoring the ARP protocol.

![Pasted image 20240327200915](https://github.com/lm3nitro/Projects/assets/55665256/de3f64f9-2f34-4363-8d51-ca27913bd238)

In the screenshot below it shows all of the ARP requests sent by 08:00:27:53:0c:ba to ff:ff:ff:ff:ff:ff:

![Pasted image 20240327202326](https://github.com/lm3nitro/Projects/assets/55665256/b380cb11-5a73-4c7e-b21a-a2ab71516b6b)

The node is flooding the network with ARP requests, seeking the MAC addresses corresponding to each IP node in the network.

![Pasted image 20240327202448](https://github.com/lm3nitro/Projects/assets/55665256/657d58ba-7fc6-4b7d-85eb-3dcd3d08c4c3)

In this scenario, we observe that IP address 192.168.10.5 is generating a significant number of requests to all network nodes with MAC addresses ending in 0c:ba. Subsequently, the same IP address masquerades as 192.168.10.1 with a MAC address ending in b9:4f. This allows the attacker to impersonate any node on the network or conduct a Man-in-the-Middle (MITM) attack, directing all traffic through the attacker's system.

![Pasted image 20240327181139](https://github.com/lm3nitro/Projects/assets/55665256/59bdf099-e010-4eb0-a54c-a5ec4bbff16d)

By actively monitoring the target connection they intend to hijack, the attacker conducts sequence number prediction to inject malicious packets in the correct order. During this injection, they spoof the source address to appear identical to our affected machine.

![Pasted image 20240327174332](https://github.com/lm3nitro/Projects/assets/55665256/43ccec86-983b-4465-aa93-2b2263860f25)

### Effect/Impact:

+ Data Breach: Sensitive information can be intercepted and exploited.
+ Unauthorized Transactions: Attackers can perform actions without the knowledge of the legitimate user, leading to fraud or loss.
+ Loss of Trust: Victims may lose trust in the security of their communications, damaging the reputation of affected services or organizations.

### Detection:

To detect TCP session hijacking via TCP header flags, it's crucial to actively monitor and analyze TCP traffic between hosts for any irregularities in the flags. For instance, unexpected or repetitive SYN, ACK, FIN, or RST flags may indicate an abnormal attempt to start, acknowledge, terminate, or reset a TCP connection without following the normal protocol. Similarly, discrepancies like mismatched or out-of-sequence sequence numbers could suggest packet injection or modification, while inconsistent or absent PSH or URG flags may signal attempts to tamper with data or packet priority. By vigilantly monitoring these TCP header flag anomalies, detecting potential malicious activities related to TCP session hijacking becomes feasible, allowing for effective protection against such attacks.

### Defense/Mitigation:

+ Implement protocols like TLS (Transport Layer Security) or SSL (Secure Sockets Layer) encrypts the data being transmitted, making it difficult for attackers to read or manipulate packets.
+ Employ strong authentication methods, such as multi-factor authentication (MFA), to verify the identities of the communicating parties.
+ IPsec can be used to encrypt and authenticate IP packets at the network layer, making it difficult for attackers to intercept or manipulate data during transmission.
+ Implement NAC solutions to ensure that only authorized devices can connect to the network, reducing the risk of an attacker gaining access to network traffic.
