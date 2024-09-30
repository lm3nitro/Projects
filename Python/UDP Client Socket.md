# Installing ncat:

![Pasted image 20240927111652](https://github.com/user-attachments/assets/19654836-31c0-49b3-ad71-3ef8d8fd4d88)

### Python code:

```
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
# IDE:
![Pasted image 20240927140645](https://github.com/user-attachments/assets/7f9b4be4-8a96-4152-97c8-3de93ff281c8)

# Creating a Netcat listener:

![Pasted image 20240927141740](https://github.com/user-attachments/assets/44870f52-d437-42f3-9e66-546fe6a0d1ff)

### Verify the listening port:

![Pasted image 20240927140828](https://github.com/user-attachments/assets/e8ccbab2-8ced-4feb-9a5e-ea7a4e728dae)


# Network traffic from client and server over UDP:

![Pasted image 20240927140423](https://github.com/user-attachments/assets/ca262138-f641-44eb-bcfd-c07da758e9ad)

Sever responses in red:

![Pasted image 20240927141817](https://github.com/user-attachments/assets/1b5bb46d-c68b-4afe-8ac9-2e1a98028b08)

# Ouput from Netcat listener:

![Pasted image 20240927140508](https://github.com/user-attachments/assets/f7b51f0e-adcb-4862-b9a3-89ebea9ef275)


