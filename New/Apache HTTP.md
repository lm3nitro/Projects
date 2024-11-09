# Apache HTTP

![Pasted image 20240921144150](https://github.com/user-attachments/assets/f088cca1-549b-43bf-8c7f-35a90adbf961)

An Apache forward proxy is a server that acts as an intermediary for client requests seeking resources from other servers. When a client makes a request, the proxy server forwards that request to the target server on behalf of the client. Once the response is received, the proxy sends it back to the client. Overall, an Apache forward proxy enhances privacy, security, and performance in web communications.

### Scope:

In this exercise, I will install and configure a forward proxy using Apache. Next, I will set up a Windows virtual machine (VM) to operate behind the proxy, ensuring it is configured to utilize the proxy server. Once that is complete, I will establish an IIS server, allowing the Windows VM client to connect to the web server. Finally, I will analyze the traffic generated on both the proxy and the web server.

Here is a view of the topology:

![Pasted image 20240921144953](https://github.com/user-attachments/assets/480e48c9-8ede-45db-a953-13aea0928380)

### Tools and Technology:

IIS Server, Apache HTTP, Win 10, Wireshark, and Linux

## Installation:

The first thing I did was install Apache:

```
apt install apache2
```

![Pasted image 20240921130542](https://github.com/user-attachments/assets/c64c0f2f-5cd1-4aaa-b0a3-10ea1efd6010)

Enabled the service:

```
systemctl enable apache2
```

![Pasted image 20240921132441](https://github.com/user-attachments/assets/a72f2b7d-8569-496f-9c89-26358d4491aa)

To enable the proxy, proxy_http, and proxy_connect modules, I used the a2enmod command:

```
a2enmod proxy 
a2enmod proxy_http
a2enmod proxy connect 
a2enmod proxy_html
```

![Pasted image 20240921130842](https://github.com/user-attachments/assets/a60614bd-7129-440e-ab45-2d5fdad03aed)


I then restarted the services:

```
systemctl restart apache2
```

Verified the service:

![Pasted image 20240921131233](https://github.com/user-attachments/assets/7e0ceaec-05a1-4aea-a017-cdc4ba27a8a4)

## Apache Configuration

In the `/etc/apache2/mods-enabled` directory I opened the file proxy.conf and uncommented the `ProxyRequests On` line and the `<Proxy *>` block:

![Pasted image 20240921131446](https://github.com/user-attachments/assets/775b9ca3-db3d-4223-9550-a269935cb2ba)

Next, I created a new file in the `/etc/apache2/sites-available` directory. I will call the file  forward_proxy.conf . This is the configuration of the file:

![Pasted image 20240921131835](https://github.com/user-attachments/assets/9e75af50-3f6e-4a70-994b-a74a1ce047d9)

I then opened the `/etc/apache2/ports.conf` file and added the `Listen 8080` line:

![Pasted image 20240921131942](https://github.com/user-attachments/assets/13a5937e-b1c5-404e-acc7-950e047e4280)

I proceeded to enable the site using the `a2ensite forward_proxy.conf` command then reloaded with `systemctl reload apache2` command:

![Pasted image 20240921132045](https://github.com/user-attachments/assets/29edfa7e-a69a-4a43-bb30-055812ea5af7)

Verified the port is listening:
 
![Pasted image 20240921132330](https://github.com/user-attachments/assets/1cd7ae60-bc3c-4820-91f0-ebaf9614a116)

## Client Proxy Configuration:

I will be using a Win 10 VM behind the proxy. I will be configuring it to use the proxy. To do this, I navigated to the `Connection Settings` and entered the information regarding the proxy server previously configured:

![Pasted image 20240921142454](https://github.com/user-attachments/assets/c6c211e5-3c26-48e1-8362-aca85d50d61e)

After configuring the proxy server on the Win VM, I then opened Wireshark to capture traffic and verify that it was getting sent to the proxy on port 8080:

![Pasted image 20240921140558](https://github.com/user-attachments/assets/a1bbc789-cf13-4f0e-9f09-028b2f6cfb6c)

## IIS website loaded:

On the other end, I also stood up an IIS server:

![Pasted image 20240921144338](https://github.com/user-attachments/assets/101be7d7-3088-43ec-8948-eb54b8279693)

After setting up the IIS server, I used the Win VM to generate traffic to the IIS Web Server. 

## Apache Proxy Logs:

On the Proxy server, I could see the requests coming from the client to the IIS server:

![Pasted image 20240921141449](https://github.com/user-attachments/assets/07385d62-1601-4463-b2f3-e0682e8e65d1)

I also verified the connection state from the client and proxy:

![Pasted image 20240921144033](https://github.com/user-attachments/assets/bc3e972d-963b-4d78-96d2-c761b9eaea91)

## IIS Web Server traffic:

I also looked at the traffic on the IIS web server and was able to see traffic coming from the Proxy Server (Apache):

![Pasted image 20240921140915](https://github.com/user-attachments/assets/b0fe0cce-3781-4744-bb6f-12870a0cb97f)

I then took a look at the IIS Web Server logs:

![Pasted image 20240921141009](https://github.com/user-attachments/assets/90ce2ff5-1849-4bc2-a255-a4e7a148e4bb)

I was also able to decode the traffic coming from the 192.168.91.131 (Win vm) client. I can see that the client is learning the request via 127.0.0.1:8080:

![Pasted image 20240921142041](https://github.com/user-attachments/assets/f6eb636e-87fd-473c-b875-fd5e4b194ae7)

### Summary:

In this exercise, the installation and configuration of the Apache forward proxy were successfully completed. I was also able to set up my Windows VM behind the proxy and configured it so that it would route its traffic through it. An IIS server was also established, allowing the Windows VM client to connect and interact with the web server. The traffic generated during this process was analyzed on both the proxy and the web server, providing valuable insights into network behavior and performance.

I was able to learn about the role of a forward proxy in handling client requests and the interaction between clients, proxies, and servers. The exercise illustrates how forward proxies enhance user privacy by masking IP addresses. 

