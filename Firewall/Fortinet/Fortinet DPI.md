# Fortinet Deep Packet Inspection (DPI)

<img width="444" alt="Screenshot 2024-09-19 at 10 01 04 PM" src="https://github.com/user-attachments/assets/793a23a6-7722-42b7-9749-a61642ac095b">

DPI on Fortinet firewalls is a security feature that analyzes the content of the network packets beyond just its headers. It allows the firewall to identify and block various types of threats, including malware and intrusion attempts. 

DPI is a technology that allows for the inspection and analysis of the contents of network packets passing through a firewall or security gateway. It does more than just the traditional packet filtering by examining the actual data payload of packets, including application-layer protocols, to make informed security decisions.

### Scope: 

I currently have a Fortinet 60F firewall in my home network. The exercise involves enabling Deep Packet Inspection (DPI) on a FortiGate Firewall and installing the firewall's SSL certificate on a host to test the functionality of traffic monitoring and control.  Fortinet's DPI is a crucial component of their security solutions, providing advanced visibility and control over network traffic at a granular level. I will then test to confirm that the firewall accurately monitors and controls traffic, enhancing network security and performance.

### Tools and Technology:

Fortinet Firewall and Wireshark

## Getting Started

To start, I wanted to perform this on one particular host (10.10.100.104). When FortiGate encrypts the content again, it will use a certificate. Since Fortinet's Certificate isn't in the browser's list of trusted certificates, the browser will show a warning about an untrusted certificate when it gets a re-signed certificate from Fortinet. To avoid these warnings, I added the certificate to my trusted certificates.

Here is a view of the certificate where it displays that it was issued by the Firewall:

![Pasted image 20240408205716](https://github.com/lm3nitro/Projects/assets/55665256/3e50b6ba-2ea5-43aa-a66b-bd6ec51b831d)

This is a look at the certificate itself:

![Pasted image 20240408210241](https://github.com/lm3nitro/Projects/assets/55665256/0e9282bc-4438-4d1c-a650-58cd519ad424)

Once I had the certificate, I needed to import it on my host machine on 10.10.100.104:

![Pasted image 20240408210328](https://github.com/lm3nitro/Projects/assets/55665256/c3793602-1a0c-4be9-8486-d6148688893a)

Placed it in the Trusted Root store:

![Pasted image 20240408210429](https://github.com/lm3nitro/Projects/assets/55665256/2afe225f-b4ec-4b43-922b-bef16b50043d)

![Pasted image 20240408210447](https://github.com/lm3nitro/Projects/assets/55665256/6da1b25c-7b55-43c1-99e5-8195593bb79f)

The certificate was successfully imported:

![Pasted image 20240408210516](https://github.com/lm3nitro/Projects/assets/55665256/e6020fc3-4ff1-4b6d-a888-332ff4794c6c)

## Testing
Next, I needed to create an a SSL/SSH profile. To do this, I navigated to the SSL/SSH Profile in my Fortinet Firewall:

![Pasted image 20240408210032](https://github.com/lm3nitro/Projects/assets/55665256/9c4409a7-cfa5-4b4a-bec0-862a661836d4)

Once that was configured, I then applied it to the security policy:

![Pasted image 20240408204514](https://github.com/lm3nitro/Projects/assets/55665256/48def19c-e288-4b4c-b52c-04b78891362f)

I then tested by downloading the Eicar test file on the host where the certificate was imported:

![Pasted image 20240408203757](https://github.com/lm3nitro/Projects/assets/55665256/5403cb29-8582-4552-8d8a-067dffb57b37)

Here is the warning message received once the Eicar test file attempted to download:

![Pasted image 20240408203715](https://github.com/lm3nitro/Projects/assets/55665256/537f0304-b0f0-4f53-bf73-03bd524c67f0)

## Analyzing:

Within the Firewall, I was also able to investigate the traffic and see the following from the IP address going to the site:

![Pasted image 20240408210948](https://github.com/lm3nitro/Projects/assets/55665256/29be7fee-5420-40b8-86b8-32bc6632975f)

Here I can see the search engine and the key phrase used on the target host:

![Pasted image 20240408211125](https://github.com/lm3nitro/Projects/assets/55665256/9200e777-5fe2-4306-9234-f97ad45df5f2)

![Pasted image 20240408211335](https://github.com/lm3nitro/Projects/assets/55665256/4f1fe32d-010b-4b3f-bceb-180ee57e6087)

I was also able to see the traffic being blocked:

![Pasted image 20240408204002](https://github.com/lm3nitro/Projects/assets/55665256/a7340920-6959-444d-b150-16d5310b90cc)

![Pasted image 20240408204128](https://github.com/lm3nitro/Projects/assets/55665256/52ce4a3d-1796-415f-93b0-9d660d7cdb78)

During this test, I also had a capture running. Below is what I was able to see during the test, and as seen, it is shown in clear text:

![Pasted image 20240408204326](https://github.com/lm3nitro/Projects/assets/55665256/3a46abe3-e332-4f23-af3b-1c15bfa9fcf3)

### Summary:

Overall, Enabling Deep Packet Inspection (DPI) enhances security by allowing detailed analysis of packet contents and facilitates application layer filtering, enabling granular control over traffic and effective bandwidth management by prioritizing critical applications. This experience allowed me to gain a deeper understanding of traffic management on my host system, enabling me to effectively monitor and control the flow of data in real-time. Additionally, I learned how to configure and utilize tools like Deep Packet Inspection (DPI) and logging for enhanced threat detection and analysis. 

