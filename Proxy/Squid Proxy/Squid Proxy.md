# Squid Proxy

<img width="401" alt="Screenshot 2024-04-27 at 10 56 45 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/c1a8bcc3-1078-402d-88f2-84849ebf4ed9">

Squid Proxy is an open-source caching and forwarding HTTP proxy server. It is widely used for improving web performance, enhancing security, and controlling access to the internet. Squid supports HTTP, HTTPS, FTP, and other protocols, and is often deployed in enterprise environments, educational institutions, or as part of network appliances. 

Features:
+ Web Caching
+ Access Control and Filtering
+ Bandwidth Optimization
+ Bandwidth Optimization
+ Authentication and Logging
+ Reverse Proxy
+ Forward and Transparent Proxy and more

### Scope:

I will be installing and configuring Squid Proxy in a lab environment. I will then have another node running Win10 where I will configure it to use the newly installed Squid Proxy. This project focuses on exploring features like access control lists (ACLs), understanding how to control and manage internet access in a network, and observe web traffic in real time. By configuring domain blocking, the goal is to enforce policies that restrict access to certain websites (e.g., social media, malware sites) to improve security. I will then be sending the Squid proxy logs to Splunk for better visualization. 

### Tools and Technology:
Linux, Win10, Squid Proxy, Splunk and Wireshark

## Installing Squid Proxy

To get started, I installed Squid Proxy straight from the CLI:

```
sudo apt update
sudo apt install squid
```

![Pasted image 20240402202454](https://github.com/lm3nitro/Projects/assets/55665256/f9263069-6075-43d4-b8b0-c1839cb7b224)

Once the installation completed, the Squid service started automatically. To verify, I checked the service status:

```
sudo systemctl status squid
```

![Pasted image 20240402202521](https://github.com/lm3nitro/Projects/assets/55665256/a60b51a8-cf83-4512-9cef-9513153656b9)

## Configuring Squid

The squid service can be configured by editing the `/etc/squid/squid.conf` file. The configuration file contains comments that describe what each configuration option does. You can also put the configuration settings in separate files, which can be included in the main configuration file using the “include” directive.

Before making any changes, I created a backup of the original configuration file:

```
sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.backup
```
Next, I opened the file with the nano text editor:

```
nano /etc/squid/squid.conf
```

![Pasted image 20240402202831](https://github.com/lm3nitro/Projects/assets/55665256/b27b5d08-d539-48fe-86fc-a79d6d597409)


By default, squid is set to listen on port `3128` on all network interfaces on the server. If you want to change the port and set a listening interface, locate the line starting with `http_port` and specify the interface IP address and the new port. If no interface is specified Squid will listen on all interfaces.

![Pasted image 20240402203832](https://github.com/lm3nitro/Projects/assets/55665256/cf774d88-6f7e-4ed2-9e55-b3308edc5651)

For my project and in my lab, I am allowing traffic only from the source network 192.168.0.0/16

![Pasted image 20240402202950](https://github.com/lm3nitro/Projects/assets/55665256/6fb4dc79-e6a7-443a-9feb-dcac4a8ea873)

I saved the changes and then verified the logs from squid using the following command:

```
tail -f /var/squid/access.log
```

Squid is currently blocking all the connections:

![Pasted image 20240402203132](https://github.com/lm3nitro/Projects/assets/55665256/b4833a23-e778-457e-81e0-6d0980f8792f)

Let's allow some traffic. For testing purpose to allow all traffic, I added the following line to the configuration file: `http_access allow all`

![Pasted image 20240402203045](https://github.com/lm3nitro/Projects/assets/55665256/300cdbc2-cb3c-476c-9c67-aac92cba8a5b)

After making the change, I reloaded Squid Proxy to ensure that changes were applied:

```
systemctl restart squid
```

Squid proxy status after the restart:
```
systemctl status squid 
```

![Pasted image 20240402203538](https://github.com/lm3nitro/Projects/assets/55665256/1b67a341-b727-4465-85a4-c20e11299e48)

Taking another look at the access logs, I can see that it is no longer blocking connections:

```
tail -f /var/squid/access.log
```

![Pasted image 20240402204036](https://github.com/lm3nitro/Projects/assets/55665256/4b602451-8f87-407a-ae3d-069c338b1bad)

## Configuring proxy on Windows:

Now that the Squid Proxy is installed and configured, I need to configure my Win10 VM to use the proxy. To do this, I went to Proxy settings from the start menu:

![Pasted image 20240402205128](https://github.com/lm3nitro/Projects/assets/55665256/3c005db8-a40e-44e1-a362-d91980e1d0a2)

I manually set up the proxy with the 192.168.125.131 which is the IP for the Squid Proxy and the default port of 3128:

![Pasted image 20240402204229](https://github.com/lm3nitro/Projects/assets/55665256/450a5af5-f643-4c7c-962e-39e523f11560)

## Testing Squid Proxy

To test, I visited a non-existent website on the Win10 host:

![Pasted image 20240402204418](https://github.com/lm3nitro/Projects/assets/55665256/6650a4b6-ef28-4c1b-aebc-a1dd3cb21910)

I also verified the logs from Squid Proxy side as well:

![Pasted image 20240402204445](https://github.com/lm3nitro/Projects/assets/55665256/0873d159-5f96-4a43-a1d5-c757b30c34a4)

I also tested going to Facebook:

![Pasted image 20240402204917](https://github.com/lm3nitro/Projects/assets/55665256/d1b9ca92-5069-44a7-8fd7-3da1c1e2126a)

The Proxy logs showed that connection to Facebook:

![Pasted image 20240402205009](https://github.com/lm3nitro/Projects/assets/55665256/3bc645b4-4094-4f8c-9641-63eacdf437f7)

While going to Facebook, I also captured the traffic on Wireshark on the Win10 host and could see my IP address going to the Squid Proxy IP and that the connection was established to Facebook:

![Pasted image 20240402204836](https://github.com/lm3nitro/Projects/assets/55665256/cde41a5c-13fd-4f89-ad02-ca895de4205a)

## Domain Blacklist in Squid Proxy:

Now that I have tested Squid Proxy and can see all the traffic generated from the win10 host, I also wanted to test the domain blocking functionality that Squid offers:

To do this, I created a file in `/etc/squid/black_list.txt` 
![Pasted image 20240403143701](https://github.com/lm3nitro/Projects/assets/55665256/99923f88-8ebc-425d-b444-a5fecdb64c46)

I added the domains I wanted to block in the file: 

![Pasted image 20240403144307](https://github.com/lm3nitro/Projects/assets/55665256/ace43405-ba5e-4ec8-b921-238379477131)

```
.facebook.com
.youtube.com
.twitter.com
.fbcdn.et
```

>#### Note:  The “.” before the domain name acts as a wildcard. Example:  to block *.example.com you will type: .example.com instead.  

I saved the file. I then opened the Squid Proxy configuration file located in ` /etc/squid.conf` and added the following lines:

```
acl bad_domains dstdomain "/etc/squid/black_list.txt"
http_access deny all bad_domains
```

![Pasted image 20240403145431](https://github.com/lm3nitro/Projects/assets/55665256/916a3b19-0f91-4cac-9ac3-de27d5f806bc)

I then restarted Squid Proxy to apply the changes. 

```
systemctl restart squid
```

![Pasted image 20240403144353](https://github.com/lm3nitro/Projects/assets/55665256/198e7a47-8d02-423f-892c-867c8f44f484)

## Traffic Analysis

Now that the domains have been blocked, I tested with the Win10 host again going to both Facebook and YouTube. I checked the Squid logs after to monitor:

![Pasted image 20240403144543](https://github.com/lm3nitro/Projects/assets/55665256/a3fde7d1-0980-4511-ad08-16b93739229e)

I can see that it is blocking. I also checked Wireshark to verify and can also see that I was getting a `403 Forbidden`:

![Pasted image 20240403145119](https://github.com/lm3nitro/Projects/assets/55665256/b8a97a11-67f0-46a6-aac0-ab03754057d7)

I followed the stream, and it provided details including information about Squid:

![Pasted image 20240403145218](https://github.com/lm3nitro/Projects/assets/55665256/fd0b5cc2-b4ed-43f9-8c36-4218c02a53c7)

On the host, the browser showed the following when visiting YouTube:

![Pasted image 20240403144710](https://github.com/lm3nitro/Projects/assets/55665256/91be24a1-5228-4cce-bd32-93c53f2c31d2)

## Splunk

After doing some more testing, I also sent the Squid log traffic to the Splunk instance that I have in my lab. 

Here is the Squid index showing the logs that have been sent to Splunk:

![Pasted image 20240403161733](https://github.com/lm3nitro/Projects/assets/55665256/1dff81be-08cb-47e7-bc49-469e5350863a)

To filter for the traffic, I used the following query:

```
index="squid" src_ip=* dst_ip=* facebook | table src_ip dst_ip url method
```

![Pasted image 20240403161425](https://github.com/lm3nitro/Projects/assets/55665256/920e3811-2b97-476a-9d9d-e60880b064e2)

High overview of the available fields and information available from the Squid logs:

![Pasted image 20240403161630](https://github.com/lm3nitro/Projects/assets/55665256/42ba6d89-bc4a-47af-a38e-5839c8b716b0)

### Summary:

By installing Squid Proxy in my lab, I was able to learn how to configure and fine-tune the proxy settings to manage network traffic and domain blacklisting. The was able to gain experience with the mechanics of HTTP/HTTPS proxying and domain blocking, as well as practical skills in traffic monitoring. Additionally, I gained insight into network performance optimization techniques and the real-world application of proxy servers in both security and performance contexts. 

Testing domain blocking is important because it allowed me to simulate real-world scenarios where controlling access to certain online content is crucial. This is particularly valuable for improving network security, compliance, and productivity in environments like businesses, schools, or public networks. Domain blocking helps protect users from harmful websites, reduce distractions in work or study environments, and enforce organizational policies. I also liked the amount of information that the Squid logs offered when visualizing them using Splunk. By utilizing both Squid Proxy and Splunk, it allows users to quickly identify patterns, security incidents, or user behavior, which can be critical for troubleshooting and forensics.

In conclusion, Squid Proxy is a powerful tool for optimizing network performance, enhancing security, and managing internet access. Furthermore, its support for traffic monitoring and SSL inspection provides deep visibility into network activity, helping detect potential security threats and ensuring efficient use of resources.



