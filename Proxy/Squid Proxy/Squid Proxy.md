# $${\color{lightblue}Squid \space Proxy}$$

<img width="401" alt="Screenshot 2024-04-27 at 10 56 45 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/c1a8bcc3-1078-402d-88f2-84849ebf4ed9">

Squid Proxyis a caching and forwarding HTTP web proxy. It has a wide variety of uses, including speeding up a web server by caching repeated requests, caching World Wide Web, Domain Name System, and other network lookups for a group of people sharing network resources, and aiding security by filtering traffic.

 
## Installing Squid Proxy


sudo apt update

sudo apt install squid

![Pasted image 20240402202454](https://github.com/lm3nitro/Projects/assets/55665256/f9263069-6075-43d4-b8b0-c1839cb7b224)


Once the installation is completed, the Squid service will start automatically. To verify it, check the service status

sudo systemctl status squid

![Pasted image 20240402202521](https://github.com/lm3nitro/Projects/assets/55665256/a60b51a8-cf83-4512-9cef-9513153656b9)


## Configuring Squid

The squid service can be configured by editing the `/etc/squid/squid.conf` file. The configuration file contains comments that describe what each configuration option does. You can also put your configuration settings in separate files, which can be included in the main configuration file using the “include” directive.

Before making any changes, it is recommended to back up the original configuration file:

sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.backup


Next open the file with and text editor:

nano /etc/squid/squid.conf

![Pasted image 20240402202831](https://github.com/lm3nitro/Projects/assets/55665256/b27b5d08-d539-48fe-86fc-a79d6d597409)


By default, squid is set to listen on port `3128` on all network interfaces on the server.

If you want to change the port and set a listening interface, locate the line starting with `http_port` and specify the interface IP address and the new port. If no interface is specified Squid will listen on all interfaces.

![Pasted image 20240402203832](https://github.com/lm3nitro/Projects/assets/55665256/cf774d88-6f7e-4ed2-9e55-b3308edc5651)


Allowing traffic only from the source network 192.168.0.0/16


![Pasted image 20240402202950](https://github.com/lm3nitro/Projects/assets/55665256/6fb4dc79-e6a7-443a-9feb-dcac4a8ea873)


Let's allow some traffic. Squid is blocking all the connections:

Verify the logs from squid: tail -f /var/squid/access.log

![Pasted image 20240402203132](https://github.com/lm3nitro/Projects/assets/55665256/b4833a23-e778-457e-81e0-6d0980f8792f)


For testing purpause to allow all traffic add the following line to the configuration file: 
http_access allow all

![Pasted image 20240402203045](https://github.com/lm3nitro/Projects/assets/55665256/300cdbc2-cb3c-476c-9c67-aac92cba8a5b)


Reloading Squid proxy:

systemctl restart squid

![Pasted image 20240402203538](https://github.com/lm3nitro/Projects/assets/55665256/1b67a341-b727-4465-85a4-c20e11299e48)


Check Squid proxy status:

systemctl status squid 

![Pasted image 20240402204036](https://github.com/lm3nitro/Projects/assets/55665256/4b602451-8f87-407a-ae3d-069c338b1bad)



Configuring proxy on Windows:


![Pasted image 20240402205128](https://github.com/lm3nitro/Projects/assets/55665256/3c005db8-a40e-44e1-a362-d91980e1d0a2)


![Pasted image 20240402204229](https://github.com/lm3nitro/Projects/assets/55665256/450a5af5-f643-4c7c-962e-39e523f11560)


Tesing squid with a fake website:

![Pasted image 20240402204418](https://github.com/lm3nitro/Projects/assets/55665256/6650a4b6-ef28-4c1b-aebc-a1dd3cb21910)


Logs from Squid proxy:

![Pasted image 20240402204445](https://github.com/lm3nitro/Projects/assets/55665256/0873d159-5f96-4a43-a1d5-c757b30c34a4)



Going to Facebook via Squid proxy:


![Pasted image 20240402204917](https://github.com/lm3nitro/Projects/assets/55665256/d1b9ca92-5069-44a7-8fd7-3da1c1e2126a)



Proxy logs:

![Pasted image 20240402205009](https://github.com/lm3nitro/Projects/assets/55665256/3bc645b4-4094-4f8c-9641-63eacdf437f7)



Traffic logs:

![Pasted image 20240402204836](https://github.com/lm3nitro/Projects/assets/55665256/cde41a5c-13fd-4f89-ad02-ca895de4205a)




#  Configure Domain Blacklist in Squid:


create a file at /etc/squid/black_list.txt and add the domains you wish to block. 

Note:  The “.” before the domain name acts as a wildcard. 


![Pasted image 20240403143701](https://github.com/lm3nitro/Projects/assets/55665256/99923f88-8ebc-425d-b444-a5fecdb64c46)


Example:  to block *.example.com you will type  .example.com instead.  


![Pasted image 20240403144307](https://github.com/lm3nitro/Projects/assets/55665256/ace43405-ba5e-4ec8-b921-238379477131)


Open the configuration file of squid at /etc/squid.conf and add the following lines:

![Pasted image 20240403145431](https://github.com/lm3nitro/Projects/assets/55665256/916a3b19-0f91-4cac-9ac3-de27d5f806bc)


Restart squid:

![Pasted image 20240403144353](https://github.com/lm3nitro/Projects/assets/55665256/198e7a47-8d02-423f-892c-867c8f44f484)


systemctl restart squid




# Squid logs:



![Pasted image 20240403144543](https://github.com/lm3nitro/Projects/assets/55665256/a3fde7d1-0980-4511-ad08-16b93739229e)



# Traffic logs:

![Pasted image 20240403145119](https://github.com/lm3nitro/Projects/assets/55665256/b8a97a11-67f0-46a6-aac0-ab03754057d7)


![Pasted image 20240403145218](https://github.com/lm3nitro/Projects/assets/55665256/fd0b5cc2-b4ed-43f9-8c36-4218c02a53c7)

# Browser client :

![Pasted image 20240403144710](https://github.com/lm3nitro/Projects/assets/55665256/91be24a1-5228-4cce-bd32-93c53f2c31d2)






SPLUNK ONLY PROJECT 

##### Sending squid log traffic to Splunk


![Pasted image 20240403161733](https://github.com/lm3nitro/Projects/assets/55665256/1dff81be-08cb-47e7-bc49-469e5350863a)

![Pasted image 20240403161425](https://github.com/lm3nitro/Projects/assets/55665256/920e3811-2b97-476a-9d9d-e60880b064e2)


![Pasted image 20240403161630](https://github.com/lm3nitro/Projects/assets/55665256/42ba6d89-bc4a-47af-a38e-5839c8b716b0)
