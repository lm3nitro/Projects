## Installing PolarProxy

Create a system user for the PolarProxy daemon:  
Ubuntu:

sudo adduser --system --shell /bin/bash proxyuser

Create log directory for proxyuser:


sudo mkdir /var/log/PolarProxy  
sudo chown proxyuser:root /var/log/PolarProxy/  
sudo chmod 0775 /var/log/PolarProxy/

Download and install PolarProxy


sudo su - proxyuser
mkdir ~/PolarProxy
cd ~/PolarProxy/
curl https://www.netresec.com/?download=PolarProxy | tar -xzf -
exit

Install the systemd script for PolarProxy:


sudo cp /home/proxyuser/PolarProxy/PolarProxy.service /etc/systemd/system/PolarProxy.service

Enable and start PolarProxy service:


sudo systemctl enable PolarProxy.service  
sudo systemctl start PolarProxy.service

Check the status of PolarProxy service:


systemctl status PolarProxy.service  
journalctl -t PolarProxy



### Ubuntu certificate installation

**Import Root CA certificate to both OS and Browser**

The root CA certificate used by PolarProxy must be trusted by all clients that will have their TLS traffic routed through the proxy. Your PolarProxy root CA must be trusted by both the operating system and any browsers or applications that have their own list of trusted root certificates in order to get a seamless integration of the proxy.

In the last command we have used the switch --certhttp 10080, which will make the public root CA cert available on a web server running at the port 10080. Simply start a browser on the client and enter the IP address of PolarProxy, such as http://127.0.0.1:10080/polarproxy.cer (if started with --certhttp 10080), to access the certificate.

_Replace "192.168.242.132" below with the IP of PolarProxy._

1. wget http://192.168.242.132:10080/polarproxy.cer
2. sudo mkdir /usr/share/ca-certificates/extra
3. sudo openssl x509 -inform DER -in polarproxy.cer -out /usr/share/ca-certificates/extra/PolarProxy-root-CA.crt
4. sudo dpkg-reconfigure ca-certificates



![Pasted image 20240409205056](https://github.com/lm3nitro/Projects/assets/55665256/97ed41dd-3f34-4a30-b26b-4566c8e16fe1)

1. Select the "extra/PolarProxy-root-CA.crt" Certificate Authority
2. click OK



Checking the status of the service:

![Pasted image 20240409183458](https://github.com/lm3nitro/Projects/assets/55665256/37c9bf8f-5688-46eb-a692-d6877587e061)



Checking the listening ports:

![Pasted image 20240409183705](https://github.com/lm3nitro/Projects/assets/55665256/ee4582c3-d38c-437e-bd15-8da3487ec356)



Testing the proxy server:


![Pasted image 20240409183817](https://github.com/lm3nitro/Projects/assets/55665256/c081370f-bb71-4d05-a6c4-5bb7ee672635)


Checking the logs with journalctl to look for errors:

![Pasted image 20240409185046](https://github.com/lm3nitro/Projects/assets/55665256/aa88535e-1941-4f5f-bc4f-d3d325e12dd8)

# Redirecting traffic to the proxy:


PolarProxy is installed directly on client used for generating HTTPS traffic PolarProxy is listening on the TCP port 10443 for incoming https traffic, decrypt TLS and and save decrypted traffic to PCAP file. Encrypted traffic to port 443 is then send to a real web server. To do so, clients must be configured to redirect outgoing HTTPS traffic destined for the port TCP 443 to the port 10443, PolarProxy service is listening on.

sudo iptables -I INPUT -p tcp --dport 10443 -j ACCEPT  
sudo iptables -t nat -A OUTPUT -m owner --uid 1000 -p tcp --dport 443 -j REDIRECT --to 10443


Test run the proxy service with curl:

curl --insecure --connect-to www.netresec.com:443:127.0.0.1:10443 https://www.netresec.com/


Sniffing live traffic on the lookback interface to see if the packets are going to the proxy on 10443.


![Pasted image 20240409184100](https://github.com/lm3nitro/Projects/assets/55665256/eb18e7e0-78d7-47d4-92f1-1dda6600028b)


On the other hand the proxy is saving the encrypted packets for fruther analysis: 


I copied the pcap from the default directory: /var/log/PolarProxy/ to my home directory temporarily

![Pasted image 20240409184908](https://github.com/lm3nitro/Projects/assets/55665256/68b084e2-6038-4871-8419-a5c10998b0fc)

Un encrypted  packet capture after TLS headers have been removed: 
 
The traffic is coming from 127.0.0.1  loopback interface to www.netresec.com

![Pasted image 20240409184751](https://github.com/lm3nitro/Projects/assets/55665256/534ef2bd-ce5b-4ca3-8a27-f4e702a468ad)

Checking the Polar proxy Root certificate in firefox:

![Pasted image 20240409185332](https://github.com/lm3nitro/Projects/assets/55665256/bc313936-e861-48a6-bdb4-41f450120831)

![Pasted image 20240409185307](https://github.com/lm3nitro/Projects/assets/55665256/8bdfb19f-ff61-4009-bc19-64fd6af7a092)

# Further Testing the Proxy:

In this scenario I went to google looking for a random tree image.

![Pasted image 20240409185436](https://github.com/lm3nitro/Projects/assets/55665256/b1b90bba-7f8f-4892-860a-9efd4e0bc0bd)

The proxy has saved the traffic:

![Pasted image 20240409185821](https://github.com/lm3nitro/Projects/assets/55665256/ee85f1a8-5e2f-4d05-859b-35374609ddcc)

![Pasted image 20240409185850](https://github.com/lm3nitro/Projects/assets/55665256/66506397-cafd-4f5d-970d-8cf1a96920ab)

Analysis of the Pcap in Network minor:

![Pasted image 20240409190734](https://github.com/lm3nitro/Projects/assets/55665256/605ce843-c733-4143-ada0-64c0b2020a7c)

![Pasted image 20240409190418](https://github.com/lm3nitro/Projects/assets/55665256/9d5621d4-b84c-4c0e-999d-7daccc4fadec)

I found the image:

![Pasted image 20240409190032](https://github.com/lm3nitro/Projects/assets/55665256/5fcd7dbe-45c6-426d-90ef-a0c95a3f0bf3)

Using  Wireshark to find the image:

![Pasted image 20240409191112](https://github.com/lm3nitro/Projects/assets/55665256/c1eb5238-a47e-440a-b122-33a851a5a63b)

Exporting the image from HTTP2 trace:

![Pasted image 20240409191503](https://github.com/lm3nitro/Projects/assets/55665256/5684fe95-a684-4da0-85e7-0532ab403ccc)

![Pasted image 20240409191541](https://github.com/lm3nitro/Projects/assets/55665256/a6d21c07-d033-4375-86da-7de5dbeea955)

![Pasted image 20240409191342](https://github.com/lm3nitro/Projects/assets/55665256/7bd08c85-8358-41ae-807a-72d30e8beaf9)

Another test", going to instagram to check headers:

![Pasted image 20240409192913](https://github.com/lm3nitro/Projects/assets/55665256/f64ea7be-1e67-45bb-9f6e-c297502152b5)

![Pasted image 20240409195637](https://github.com/lm3nitro/Projects/assets/55665256/d3ceb9a4-8ffb-4d4f-98dc-2ce545aa426f)