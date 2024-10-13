
![Pasted image 20240921144150](https://github.com/user-attachments/assets/f088cca1-549b-43bf-8c7f-35a90adbf961)


Apache can be configured as both a forward and a reverse proxy. An ordinary proxy (also called a forward proxy) is an intermediate server that sits between the client and the origin server. The client is configured to use the forward proxy to access other sites. When a client want to get the content from the origin server, it sends a request to the proxy naming the origin server as the target. The proxy then requests the content from the origin server and returns it to the client.


![Pasted image 20240921144953](https://github.com/user-attachments/assets/480e48c9-8ede-45db-a953-13aea0928380)

### Installation:

apt install apache2

![Pasted image 20240921130542](https://github.com/user-attachments/assets/c64c0f2f-5cd1-4aaa-b0a3-10ea1efd6010)



systemctl enable apache2

![Pasted image 20240921132441](https://github.com/user-attachments/assets/a72f2b7d-8569-496f-9c89-26358d4491aa)


To enable the proxy, proxy_http, and proxy_connect modules. We can do that using the a2enmod command

a2enmod proxy 
a2enmod proxy_http
a2enmod proxy connect 
a2enmod proxy_html

![Pasted image 20240921130842](https://github.com/user-attachments/assets/a60614bd-7129-440e-ab45-2d5fdad03aed)


Restart services:

systemctl restart apache2


Check the services:

![Pasted image 20240921131233](https://github.com/user-attachments/assets/7e0ceaec-05a1-4aea-a017-cdc4ba27a8a4)



On /etc/apache2/mods-enabled directory and open the file proxy.conf in a text editor of your choice. Uncomment the ProxyRequests On line and the <Proxy *> block:

![Pasted image 20240921131446](https://github.com/user-attachments/assets/775b9ca3-db3d-4223-9550-a269935cb2ba)


Now, create a new file in the /etc/apache2/sites-available directory. We will call our file forward_proxy.conf. This is the configuration of the file:

![Pasted image 20240921131835](https://github.com/user-attachments/assets/9e75af50-3f6e-4a70-994b-a74a1ce047d9)

Next, open the /etc/apache2/ports.conf file and add the Listen 8080 line:

![Pasted image 20240921131942](https://github.com/user-attachments/assets/13a5937e-b1c5-404e-acc7-950e047e4280)


Enable the site using the a2ensite forward_proxy.conf  command then reload with systemctl reload apache2 command

![Pasted image 20240921132045](https://github.com/user-attachments/assets/29edfa7e-a69a-4a43-bb30-055812ea5af7)

 Verify the port is listening:
 
![Pasted image 20240921132330](https://github.com/user-attachments/assets/1cd7ae60-bc3c-4820-91f0-ebaf9614a116)


### Client proxy configuration:

![Pasted image 20240921142454](https://github.com/user-attachments/assets/c6c211e5-3c26-48e1-8362-aca85d50d61e)

### Client sending traffic to Apache proxy:

Traffic send to the proxy on port 8080

![Pasted image 20240921140558](https://github.com/user-attachments/assets/a1bbc789-cf13-4f0e-9f09-028b2f6cfb6c)


### ISS website loaded:

![Pasted image 20240921144338](https://github.com/user-attachments/assets/101be7d7-3088-43ec-8948-eb54b8279693)



### Apache Proxy server logs:

Requests coming from to the client to the IIS server.

![Pasted image 20240921141449](https://github.com/user-attachments/assets/07385d62-1601-4463-b2f3-e0682e8e65d1)

connection state from the client and proxy

![Pasted image 20240921144033](https://github.com/user-attachments/assets/bc3e972d-963b-4d78-96d2-c761b9eaea91)


### IIS Web Server traffic:

Traffic coming from the Proxy server (Apche)

![Pasted image 20240921140915](https://github.com/user-attachments/assets/b0fe0cce-3781-4744-bb6f-12870a0cb97f)


### IIS Web Server logs:


![Pasted image 20240921141009](https://github.com/user-attachments/assets/90ce2ff5-1849-4bc2-a255-a4e7a148e4bb)


### IIS Client Web Traffic:

 
Decoded traffic coming from the 192.168.91.131 client. We can see that the client is learning the request via 127.0.0.1:8080

![Pasted image 20240921142041](https://github.com/user-attachments/assets/f6eb636e-87fd-473c-b875-fd5e4b194ae7)



