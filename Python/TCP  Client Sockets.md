Downloading the Apache server:

![Pasted image 20240927111934](https://github.com/user-attachments/assets/796862c6-f1ba-4d8f-b8a5-36e8b3398f93)


Starting and checking the status of the server:

![Pasted image 20240927112051](https://github.com/user-attachments/assets/43cc243e-c349-486d-811c-66efe41d9144)

### Creating a local DNS entry for lm3nitro:

![Pasted image 20240927113041](https://github.com/user-attachments/assets/5c7286cb-99af-4684-ad39-b6433cc77b62)


Python’s socket module provides an interface to the Berkeley sockets API:

The primary socket API functions and methods in this module are:

- `socket()`
- `.bind()`
- `.listen()`
- `.accept()`
- `.connect()`
- `.connect_ex()`
- `.send()`
- `.recv()`
- `.close()`


# Python code:

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

# IDE:
![Pasted image 20240927133621](https://github.com/user-attachments/assets/0842d1d9-de6b-49bf-9a5c-1a2fcdc835ac)

# Network traffic: 

![Pasted image 20240927133842](https://github.com/user-attachments/assets/239a0483-bee2-4072-99fc-c19a98f1cd8d)


![Pasted image 20240927133925](https://github.com/user-attachments/assets/0400f08d-aefc-4866-8c42-5091acc2db28)

# Server logs:

![Pasted image 20240927133708](https://github.com/user-attachments/assets/a217d08e-1977-4538-a066-046178955386)
