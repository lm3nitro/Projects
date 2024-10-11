# DDOS Attack

A DDoS (Distributed Denial of Service) attack is a malicious attempt to disrupt the normal functioning of a targeted server, service, or network by overwhelming it with a flood of internet traffic. In a DDoS attack, multiple compromised computers (often part of a botnet) are used to send an enormous volume of requests or data to the target, making it unable to respond to legitimate traffic. This can result in slow performance, outages, or complete service disruption for users. DDoS attacks can target various types of services, including websites, online games, and APIs.

### Scope:

This scenario involves setting up a Windows 7 host as the target for a simulated DDoS attack using High Orbit Ion Cannon (HOIC), Low Orbit Ion Cannon (LOIC) and Hping 3. The objective is to emulate a realistic DDoS attack to understand its effects on system performance and service availability. HOIC, LOIC, and Hping 3 will be used to generate HTTP flood requests targeting the Windows 7 host. After executing the attack, I will analyze the traffic patterns and system response to identify vulnerabilities and the extent of disruption. Following this analysis, I will deploy Anti-DDoS Guardian to implement mitigation strategies to restore service and improve the resilience of the Windows host against future attacks. 

### Tools and Technologies:
Win7, Linux, High Orbit Ion Cannon, Low Orbit Ion Cannon, Hping 3, Wireshark, and Anti-DDOS Guardian

### Attacking:

1. High Orbit Ion Cannon
   
To Similate the DDOS attack I will start with using the High Orbit Ion Cannon. Here I have entered the information for the target host which is a Windows 7 VM:

![Pasted image 20240503091455](https://github.com/lm3nitro/Projects/assets/55665256/781de2ba-c065-4472-a897-5c3e69b1b667)

Once the target host information is inserted, I fired the lazer:

![Pasted image 20240503091544](https://github.com/lm3nitro/Projects/assets/55665256/dc09773e-1713-4ff3-bfda-82cbe60c8506)

I ensured that the parameters were set to high and selected a booster:

![Pasted image 20240503092545](https://github.com/lm3nitro/Projects/assets/55665256/86222d24-c8b6-4e6b-82c3-47142158b3b8)

2. I also used Low Orbit Ion Cannon, here are the parameters that I set:

![Pasted image 20240503092027](https://github.com/lm3nitro/Projects/assets/55665256/cfbcfd8c-baa2-4ca8-ada1-c9ed5e22998f)

3. I then used Hping 3:

![Pasted image 20240503093330](https://github.com/lm3nitro/Projects/assets/55665256/8249fed6-d648-4db3-952a-41e59e9ea3fb)

![Pasted image 20240505172107](https://github.com/lm3nitro/Projects/assets/55665256/df3fcb4c-9030-4e8c-a415-a7626340f072)

## Analyzing the target host:

On the Windows 7 VM, I had Wireshark running so that I could monitor and see the attack in real time. Here I can see the syn-flood that was being done with the Hoing 3 command above: 

![Pasted image 20240505173359](https://github.com/lm3nitro/Projects/assets/55665256/83141334-36d3-422e-ad3c-5e99b4fce58b)

Lookign at the resources for the Win 7 VM, CPU was at 76%:

![Pasted image 20240505172038](https://github.com/lm3nitro/Projects/assets/55665256/cf830ac8-f65a-4c99-ba46-2ddd05b9ce6b)

It then increased to 100%:

![Pasted image 20240503092135](https://github.com/lm3nitro/Projects/assets/55665256/c2ceeab3-717e-4d4c-965d-3a91ebdf868f)

## DDOS Attack

I then stopped that attack and wanted to simulate another DDOS attack by creating a random source address and flooding the network:

![Pasted image 20240503094321](https://github.com/lm3nitro/Projects/assets/55665256/bce43e23-1785-4b3f-811f-31b06369f2f0)

Taking a look at Wireshark while the attack is running:

![Pasted image 20240503094537](https://github.com/lm3nitro/Projects/assets/55665256/2ec8bcc7-7537-49b3-899d-123d30cefb5c)

CPU service on the target VM:

![Pasted image 20240503094243](https://github.com/lm3nitro/Projects/assets/55665256/43bee467-46b4-47d8-bda7-5fa949089c0b)

## Mitigation:

In order to mitigate these attacks, I installed the Anti-DDOS Guardian on the Win 7 VM. 

![Pasted image 20240505170626](https://github.com/lm3nitro/Projects/assets/55665256/2a10b2bf-8ff9-407e-aec7-6800d7a62040)

Selected Next:

![Pasted image 20240505170702](https://github.com/lm3nitro/Projects/assets/55665256/f240ca59-4f1e-499b-938c-a1057088424b)

Accepted the License and Terms:

![Pasted image 20240505170726](https://github.com/lm3nitro/Projects/assets/55665256/8aa072b7-f8a2-40d8-bcca-a06957e32d8e)

![Pasted image 20240505170747](https://github.com/lm3nitro/Projects/assets/55665256/14a43935-2df2-4620-820e-449287067a67)

Selected the location where it would be installed:

![Pasted image 20240505170813](https://github.com/lm3nitro/Projects/assets/55665256/5ec5349b-b54e-4d16-9892-18651c7bf206)

![Pasted image 20240505170839](https://github.com/lm3nitro/Projects/assets/55665256/bb1b8026-e1bc-441e-bfba-025a9dfcf0da)

Also installed  `Stop RDP Brute Force`:

![Pasted image 20240505170906](https://github.com/lm3nitro/Projects/assets/55665256/2a0ede17-4cc8-4db5-8b22-0208869051d7)

These are optional:

![Pasted image 20240505170933](https://github.com/lm3nitro/Projects/assets/55665256/47bce11b-95b8-4922-9247-163230086e65)

Installed:

![Pasted image 20240505170955](https://github.com/lm3nitro/Projects/assets/55665256/2a565ada-a63e-48e3-a251-f3af275e1e45)

I then Launched upon completion:

![Pasted image 20240505171142](https://github.com/lm3nitro/Projects/assets/55665256/7ff95fea-f927-4184-85fe-550b2c274497)

The first thing that I chose was to limit the amount of connections. Implementing rate limits on requests helps to control the flow of traffic. This means that even if an attacker tries to flood the server, their requests will be restricted, reducing the overall impact.

![Pasted image 20240505171335](https://github.com/lm3nitro/Projects/assets/55665256/7f12ac10-7c15-4cc7-b8af-34fccf777e18)

I then configured other perameters such as bandwidth, UDP and ICMP packets per second:

![Pasted image 20240505171408](https://github.com/lm3nitro/Projects/assets/55665256/6e95892c-e215-4a88-9c37-7424179e8b1c)

TCP half open connection setting. Limiting TCP half-open connections is an effective strategy for mitigating certain types of DDoS attacks, particularly SYN floods:

![Pasted image 20240505171433](https://github.com/lm3nitro/Projects/assets/55665256/02c44957-ed0c-44a2-95b8-bdfe05d9cd48)

Maximum number of concurrent IP addresses:

![Pasted image 20240505171503](https://github.com/lm3nitro/Projects/assets/55665256/70952cc9-bd3e-4ab5-976d-6240720b8623)

![Pasted image 20240505171522](https://github.com/lm3nitro/Projects/assets/55665256/02e9c72e-4173-4171-abe9-fbb800a8289f)

## Testing Mitigation:

Once the Anti-DDOS Guardian was installed and configured on the target host, I wanted to test its effectiveness. To start, I performed another DoS attack using Hping 3:

![Pasted image 20240505173923](https://github.com/lm3nitro/Projects/assets/55665256/20774dfe-0fbf-4212-bce7-792c0c49222e)

By doing this, here I can see that the CPU rises to 56% but it has significantly decreased after implementing the mitigation:

![Pasted image 20240505173859](https://github.com/lm3nitro/Projects/assets/55665256/bc21365c-a951-42f6-8a17-067060a1c70e)

The System Idle Process is a system-level process in Windows operating systems that indicates the percentage of CPU resources that are not being used. Here I was able to see that it was at 97%:

![Pasted image 20240505174217](https://github.com/lm3nitro/Projects/assets/55665256/cf774d4a-4cea-4e85-b649-1a4b2a512994)

I then ran another test using Hping to flood the target VM:

![Pasted image 20240505175619](https://github.com/lm3nitro/Projects/assets/55665256/40c232e9-f3df-4478-b544-ff4923067f01)

Lookig at the interface for the Anti-DDOS Guardian, I was able to see the number of bytes that it was blocking from the attacking host:

![Pasted image 20240505175527](https://github.com/lm3nitro/Projects/assets/55665256/9344f369-bd9b-47a4-bcd9-c41e7323c157)

It also allows you to expand to see more details on the blocks it is performing. I was able to see the details and it matches the command that i used above to perform the attack:

![Pasted image 20240505180225](https://github.com/lm3nitro/Projects/assets/55665256/633a290f-5cfa-4733-ac44-303738b62ec0)

Taking another look at the CPU, it is looking much better than originally when it was at 100%:

![Pasted image 20240505175809](https://github.com/lm3nitro/Projects/assets/55665256/ec12c831-57d8-4839-a27a-7f5f741f3036)

### Summary:

In this attack simulation, I was able to cover attack detection, traffic analysis, and mitigation techniques, which enhanced my understanding of DDoS vulnerabilities and defenses. I was also able to see how easily systems can be overwhelmed and the importance of analyzing traffic patterns for effective mitigation strategies. By utilizing tools like Anti-DDoS Guardian and enforcing connection limits, I was able to mitigate against the DDOS attack which ensures availability. 

By simulating the different attacks myself, I was able to learn more anout the techniques, tools, and methodologies that attackers use. The exercise highlights the importance of understanding attack vectors and their impacts on system availability, which is critical for maintaining the confidentiality, integrity, and availability (CIA) of information systems.

This attack simulation ultimately emphasized the need for a comprehensive security posture that not only safeguards data confidentiality and integrity but also prioritizes system availability.
