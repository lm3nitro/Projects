# Client and Server Sockets

In networking, server and client sockets are the endpoints used by programs to communicate over a network. They are essential for establishing a network connection and exchanging data between two endpoints (or processes) over a network.

### Scope:

I have created 3 python scripts where I will be creating 3 sockets:

+ TCP Client Socket
+ TCP Server Socket
+ UDP Client Socket

A server socket waits for incoming connections, while a client socket connects to a server to request data or services. Creating and understanding how sockets work is important because network communication-based attacks are at the heart of many types of attacks.

Understanding and creating sockets has the following benefits:

+ Attack Detection and Prevention: Knowing how server and client sockets work can help to detect and mitigate attacks like DDoS, MITM, botnets, and remote access threats.
+ Secure Communication: Ensuring data is securely transmitted over sockets, with encryption and authentication, is key to protecting sensitive information.
+ Incident Response: In post-breach analysis, understanding socket connections helps trace attackers' actions and block future intrusions.
+ Penetration Testing: Creating sockets can be used as a way to simulate attacks, find vulnerabilities, and fix them before attackers exploit them.
+ Traffic Monitoring and Firewall Configuration: Knowledge of how sockets work helps configure network defenses like firewalls and IDS systems to spot malicious activities.

### Tools and Technology:
Python, Linux, and Apache2

<details><summary>TCP Client Socket</summary>

The following script I created will create a TCP Client Socket. However, before creating it, I needed a way to test. To do this, I downloaded and installed Apache2:

![Pasted image 20240927111934](https://github.com/user-attachments/assets/796862c6-f1ba-4d8f-b8a5-36e8b3398f93)

Starting and checking the status of the server:

![Pasted image 20240927112051](https://github.com/user-attachments/assets/43cc243e-c349-486d-811c-66efe41d9144)

Creating a local DNS entry for lm3nitro:

![Pasted image 20240927113041](https://github.com/user-attachments/assets/5c7286cb-99af-4684-ad39-b6433cc77b62)

Python’s socket module provides an interface to the Berkeley sockets API. The primary socket API functions and methods in this module are:

+ `socket()`
+ `.bind()`
+ `.listen()`
+ `.accept()`
+ `.connect()`
+ `.connect_ex()`
+ `.send()`
+ `.recv()`
+ `.close()`

### Script:

```python
  
# Importing the socket module  
import socket  
# Creating two variables to server and port information  
server = "lm3nitro.com"  
port = 80  
# Groping variables  
server_info = (server, port)  
# Creating a client socket object to communicate to the server  
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
# connecting  the client to server  
client.connect((server_info))  
# Client sending HTTP GET request to server  
# \r stands for “carriage return” (ASCII 13) while \n stands for “linefeed” (ASC10)  
client.send(b"GET / HTTP/1.1\r\nHost: lm3nitro.com\r\n\r\nUser-Agent: Mozilla/lm3nitro 1.0rv:130.0)\r\n\r\n")  
# Client receive 65635 bytes of data from server  
server_response = client.recv(8192)  
# Show the responses from the server in human readable  
print(server_response.decode())  
#Closing the connection for the client  
client.close()
```

A view of the script in IDE:

![Pasted image 20240927133621](https://github.com/user-attachments/assets/0842d1d9-de6b-49bf-9a5c-1a2fcdc835ac)

I then tested and ran the script. While doing so, I had Wireshark running in order to capture and analyze the traffic:

![Pasted image 20240927133842](https://github.com/user-attachments/assets/239a0483-bee2-4072-99fc-c19a98f1cd8d)

As part of the script, I also modified my user agent to reflect 'lm3nitro', this was also captured in Wireshark:

![Pasted image 20240927133925](https://github.com/user-attachments/assets/0400f08d-aefc-4866-8c42-5091acc2db28)

I then checked the Apache2 server logs and could also see the information reflecting my user agent and the connection:

![Pasted image 20240927133708](https://github.com/user-attachments/assets/a217d08e-1977-4538-a066-046178955386)

</details>

<details><summary>TCP Server Socket</summary>

I then created a script, this time to create a TCP server socket. Below is the script:

```python
import socket  
import threading  
  
IP = "127.0.0.1"  
PORT = 9999  
def main():  
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
    server.bind((IP,PORT))  
    server.listen(5)  
    print(f'[*] Listening on {IP}:{PORT}')  
  
    while True:  
        client, address = server.accept()  
        print(f'[*] Accepted connection from {address[0]}:{address[1]}')  
        client_handler = threading.Thread(target=handle_client, args=(client,))  
        client_handler.start()  
  
def handle_client(client_socket):  
    with client_socket as sock:  
        request = sock.recv(4096)  
        print(f"[*] Received: {request.decode('utf-8')}")  
        sock.send(b'lm3nitro server over TCP is working')  
  
if __name__ == '__main__':  
    main()
```

A view of the script in the IDE:

![Pasted image 20240927162425](https://github.com/user-attachments/assets/b5cbd745-6184-4a46-8174-93fdb2bf7db2)

Next, I ran both the client and server scripts:

![Pasted image 20240927162545](https://github.com/user-attachments/assets/41c35a8c-36f1-4f6c-9282-49f1d0575df6)

This is the Wireshark view of the network traffic from client to server:

![Pasted image 20240927162349](https://github.com/user-attachments/assets/f63e843c-9a9a-4009-bc32-2381fd36d80e)

Another view of the user agent modification:

![Pasted image 20240927162647](https://github.com/user-attachments/assets/b025888d-8f0f-4780-9255-9f7ef6e7044c)

</details>

<details><summary>UDP Client Socket</summary>

Lastly, I created a script in order to create a UDP client socket. In order to test, I will be using ncat. Before testing I installed ncat:
  
```
sudo apt install ncat
```

![Pasted image 20240927111652](https://github.com/user-attachments/assets/19654836-31c0-49b3-ad71-3ef8d8fd4d88)

Below is a view of the script:

```python
# Importing the socket module  
import socket  
# Creating two variables to server and port information  
server = "lm3nitro.com"  
port = 8080  
# Groping variables  
server_info = (server, port)  
# Creating client socket object to communicate with server  
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  
# Client sending some data to server  
client.sendto(b"Hello udp lm3nitro.com server",(server_info))  
# Client receive 65635 bytes of data from server  
data, addr = client.recvfrom(4096)  
# Show the responses from the server in human readable  
print(data.decode())  
#Closing the connection for the client  
client.close()
```
The script in IDE:

![Pasted image 20240927140645](https://github.com/user-attachments/assets/7f9b4be4-8a96-4152-97c8-3de93ff281c8)

Next, I created a Ncat listener on port 8080:

![Pasted image 20240927141740](https://github.com/user-attachments/assets/44870f52-d437-42f3-9e66-546fe6a0d1ff)

I also verified the listening port:

![Pasted image 20240927140828](https://github.com/user-attachments/assets/e8ccbab2-8ced-4feb-9a5e-ea7a4e728dae)

After confirming, I then executed the script. Below is the network traffic from client and server over UDP in Wireshark:

![Pasted image 20240927140423](https://github.com/user-attachments/assets/ca262138-f641-44eb-bcfd-c07da758e9ad)

Sever responses are red and client in blue:

![Pasted image 20240927141817](https://github.com/user-attachments/assets/1b5bb46d-c68b-4afe-8ac9-2e1a98028b08)

Below is the ouptut from the Ncat listener:

![Pasted image 20240927140508](https://github.com/user-attachments/assets/f7b51f0e-adcb-4862-b9a3-89ebea9ef275)

</details>

### Summary:

By creating the scripts above, testing that it works, and verifying the communication in Wireshark I was able to gain expeience using the Python socket library and better understanding on how both client and servers function. I was able to observe the flow of data between the client and server and see how network protocols like TCP and UDP handle communication. Capturing the traffic using Wireshark also allowed me to validate that the communication was occurring correctly. Overall, this provided me with practical insight into both the coding and network analysis aspects of socket programming. 
