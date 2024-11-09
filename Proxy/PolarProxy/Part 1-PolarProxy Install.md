# PolarProxy

![Screenshot 2024-04-27 at 10 42 13 PM](https://github.com/lm3nitro/Projects/assets/55665256/4ad37b63-2784-4fea-99bd-b3c9bf577b82)

PolarProxy is a transparent TLS proxy developed by Netresec, primarily used for inspecting and analyzing encrypted network traffic (HTTPS traffic, TLS, and SSL connections). Its main purpose is to decrypt TLS/SSL traffic and make it available for analysis without altering the traffic or requiring access to the private keys from the servers involved in the encrypted communication. Below are some of the key features:

+ Decryption of TLS/SSL Traffic
+ PCAP Logging
+ Supports Different TLS Versions
+ Transparent Proxy

### Scope:

This is a 2-part project. This is part one and will cover the installation and configuraiton of PolarProxy on a VM running Ubuntu. The goal is to create a controlled environment in a lab setting where PolarProxy is used to intercept, decrypt, and analyze TLS/SSL traffic. The key focus is to understand how encrypted traffic can be inspected while maintaining the integrity of communication.

### Tools and Technology:

Ubuntu, Wireshark and PolarProxy

## Installing PolarProxy

To get started, I created a system user for the PolarProxy daemon:

```
sudo adduser --system --shell /bin/bash proxyuser
```

I then created a log directory for the proxyuser:

```
sudo mkdir /var/log/PolarProxy  
sudo chown proxyuser:root /var/log/PolarProxy/  
sudo chmod 0775 /var/log/PolarProxy/
```

Mext, I downloaded and installed PolarProxy:

```
sudo su - proxyuser
mkdir ~/PolarProxy
cd ~/PolarProxy/
curl https://www.netresec.com/?download=PolarProxy | tar -xzf -
exit
```

I installed the systemd script for PolarProxy:

```
sudo cp /home/proxyuser/PolarProxy/PolarProxy.service /etc/systemd/system/PolarProxy.service
```

Enabled and start PolarProxy service:

```
sudo systemctl enable PolarProxy.service  
sudo systemctl start PolarProxy.service
```

Checked the status of PolarProxy service:

```
systemctl status PolarProxy.service  
journalctl -t PolarProxy
```

## Certificate Installation

Next, I needed to import the root CA certificate to both OS and browser. In order to use PolarProxy effectively, the root CA certificate it uses must be trusted by all clients whose TLS traffic goes through the proxy. This means your PolarProxy root CA needs to be trusted by both the operating system and any browsers or applications that have their own list of trusted root certificates. This ensures a smooth integration of the proxy with your system and applications.

In the command below, you will see I have used the switch --certhttp 10080, this will make the public root CA cert available on a web server running at the port 10080. Simply start a browser on the client and enter the IP address of PolarProxy, such as http://127.0.0.1:10080/polarproxy.cer (if started with --certhttp 10080), to access the certificate.

_Replace "192.168.242.132" below with the IP of PolarProxy._

```
wget http://192.168.242.132:10080/polarproxy.cer
sudo mkdir /usr/share/ca-certificates/extra
sudo openssl x509 -inform DER -in polarproxy.cer -out /usr/share/ca-certificates/extra/PolarProxy-root-CA.crt
sudo dpkg-reconfigure ca-certificates
```

![Pasted image 20240409205056](https://github.com/lm3nitro/Projects/assets/55665256/97ed41dd-3f34-4a30-b26b-4566c8e16fe1)

*Select the "extra/PolarProxy-root-CA.crt" Certificate Authority then, Click OK*

Checking the status of the service:

![Pasted image 20240409183458](https://github.com/lm3nitro/Projects/assets/55665256/37c9bf8f-5688-46eb-a692-d6877587e061)

Checking the listening ports:

![Pasted image 20240409183705](https://github.com/lm3nitro/Projects/assets/55665256/ee4582c3-d38c-437e-bd15-8da3487ec356)

Testing the proxy server:

![Pasted image 20240409183817](https://github.com/lm3nitro/Projects/assets/55665256/c081370f-bb71-4d05-a6c4-5bb7ee672635)

Checking the logs with journalctl to look for errors:

![Pasted image 20240409185046](https://github.com/lm3nitro/Projects/assets/55665256/aa88535e-1941-4f5f-bc4f-d3d325e12dd8)


## Redirecting traffic to the proxy:

By default, PolarProxy listens on TCP port 10443 for incoming HTTPS traffic. When it receives this traffic, it decrypts the TLS and saves the decrypted data to a pcap file. Encrypted traffic intended for port 443 is then forwarded to a genuine web server. To enable this functionality, clients need to be configured to redirect outgoing HTTPS traffic meant for TCP port 443 to PolarProxy's listening port, which is 10443.

In order to do this, these are the steps I followed:

```
sudo iptables -I INPUT -p tcp --dport 10443 -j ACCEPT  
sudo iptables -t nat -A OUTPUT -m owner --uid 1000 -p tcp --dport 443 -j REDIRECT --to 10443
```

Tested the proxy with curl:
```
curl --insecure --connect-to www.netresec.com:443:127.0.0.1:10443 https://www.netresec.com/
```

I wanted to sniff live traffic on the lookback interface to see if the packets are going to the proxy on port 10443.

![Pasted image 20240409184100](https://github.com/lm3nitro/Projects/assets/55665256/eb18e7e0-78d7-47d4-92f1-1dda6600028b)

At the same time, PolarProxy is also saving the encrypted packets for further analysis. I copied the pcap from the default directory: /var/log/PolarProxy/ to my home directory temporarily.

![Pasted image 20240409184908](https://github.com/lm3nitro/Projects/assets/55665256/68b084e2-6038-4871-8419-a5c10998b0fc)

After, I wanted to take a look at the unencrypted pcap after the TLS headers had been removed. Here we see the traffic is coming from the 127.0.0.1 loopback interface to www.netresec.com:

![Pasted image 20240409184751](https://github.com/lm3nitro/Projects/assets/55665256/534ef2bd-ce5b-4ca3-8a27-f4e702a468ad)

Lets verify that the Polar proxy Root certificate is indeed in our Firefox browser:

![Pasted image 20240409185332](https://github.com/lm3nitro/Projects/assets/55665256/bc313936-e861-48a6-bdb4-41f450120831)

![Pasted image 20240409185307](https://github.com/lm3nitro/Projects/assets/55665256/8bdfb19f-ff61-4009-bc19-64fd6af7a092)

### Summary:

By installing PolarProxy, I gained practical experience in setting up a transparent proxy and managing SSL/TLS certificates for encrypted traffic interception. Even without diving into deep analysis, I was able to capture and log network traffic, helping me understand packet flow and secure communication. This installation laid the groundwork for developing more advanced skills in traffic decryption, network security, and forensics.

In part 2 I will be using PolarProxy along with NetworkMiner to test and analyze the decryption of traffic.  
