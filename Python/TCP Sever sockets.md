### Python code:

```
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

# IDE:

![Pasted image 20240927162425](https://github.com/user-attachments/assets/b5cbd745-6184-4a46-8174-93fdb2bf7db2)


### Running the client and server script:


![Pasted image 20240927162545](https://github.com/user-attachments/assets/41c35a8c-36f1-4f6c-9282-49f1d0575df6)


# Network traffic from client to server:

![Pasted image 20240927162349](https://github.com/user-attachments/assets/f63e843c-9a9a-4009-bc32-2381fd36d80e)


![Pasted image 20240927162647](https://github.com/user-attachments/assets/b025888d-8f0f-4780-9255-9f7ef6e7044c)
