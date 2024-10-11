# DDOS Attack

A DDoS (Distributed Denial of Service) attack is a malicious attempt to disrupt the normal functioning of a targeted server, service, or network by overwhelming it with a flood of internet traffic. In a DDoS attack, multiple compromised computers (often part of a botnet) are used to send an enormous volume of requests or data to the target, making it unable to respond to legitimate traffic. This can result in slow performance, outages, or complete service disruption for users. DDoS attacks can target various types of services, including websites, online games, and APIs.

### Scope:

This scenario involves setting up a Windows 7 host as the target for a simulated DDoS attack using High Orbit Ion Cannon (HOIC), Low Orbit Ion Cannon (LOIC) and hping. The objective is to emulate a realistic DDoS attack to understand its effects on system performance and service availability. HOIC and Hping will be used to generate HTTP flood requests targeting the Windows 7 host. After executing the attack, I will analyze the traffic patterns and system response to identify vulnerabilities and the extent of disruption. Following this analysis, I will deploy Anti-DDoS Guardian to implement mitigation strategies to restore service and improve the resilience of the Windows host against future attacks. 

### Tools and Technologies:
Win7, Linux, High Orbit Ion Cannon, Low Orbit Ion Cannon, Hping, Wireshark, and Anti-DDOS Guardian

### Getting Started:

To Similate the DDOS attack I will start with using the High Orbit Ion Cannon. Here I have entered the information for the target host which is a Windows 7 VM:

![Pasted image 20240503091455](https://github.com/lm3nitro/Projects/assets/55665256/781de2ba-c065-4472-a897-5c3e69b1b667)

Once the target host information is inserted, I fired the lazer:

![Pasted image 20240503091544](https://github.com/lm3nitro/Projects/assets/55665256/dc09773e-1713-4ff3-bfda-82cbe60c8506)

I ensured that the parameters were set to high and selected a booster:

![Pasted image 20240503092545](https://github.com/lm3nitro/Projects/assets/55665256/86222d24-c8b6-4e6b-82c3-47142158b3b8)


Using Low Orbit:

![Pasted image 20240503092027](https://github.com/lm3nitro/Projects/assets/55665256/cfbcfd8c-baa2-4ca8-ada1-c9ed5e22998f)


Using Hping 3 to create a PoD attack:

In this attack the attacker tries to crash, freeze, or destabilize the targeted system or service by sending malformed or oversized packets

![Pasted image 20240503093330](https://github.com/lm3nitro/Projects/assets/55665256/8249fed6-d648-4db3-952a-41e59e9ea3fb)


DoS attack with hping3

![Pasted image 20240505172107](https://github.com/lm3nitro/Projects/assets/55665256/df3fcb4c-9030-4e8c-a415-a7626340f072)

Spoof traffic going to the windows 7 target:

![Pasted image 20240505173359](https://github.com/lm3nitro/Projects/assets/55665256/83141334-36d3-422e-ad3c-5e99b4fce58b)

on the windows 7 target:

![Pasted image 20240505172038](https://github.com/lm3nitro/Projects/assets/55665256/cf830ac8-f65a-4c99-ba46-2ddd05b9ce6b)

![Pasted image 20240503092135](https://github.com/lm3nitro/Projects/assets/55665256/c2ceeab3-717e-4d4c-965d-3a91ebdf868f)


Simulating a DDOS attack by creating a random source address and flooding the  network


![Pasted image 20240503094321](https://github.com/lm3nitro/Projects/assets/55665256/bce43e23-1785-4b3f-811f-31b06369f2f0)

![Pasted image 20240503094537](https://github.com/lm3nitro/Projects/assets/55665256/2ec8bcc7-7537-49b3-899d-123d30cefb5c)

![Pasted image 20240503094243](https://github.com/lm3nitro/Projects/assets/55665256/43bee467-46b4-47d8-bda7-5fa949089c0b)

Anti DDoS:

![Pasted image 20240505170626](https://github.com/lm3nitro/Projects/assets/55665256/2a10b2bf-8ff9-407e-aec7-6800d7a62040)

![Pasted image 20240505170702](https://github.com/lm3nitro/Projects/assets/55665256/f240ca59-4f1e-499b-938c-a1057088424b)

![Pasted image 20240505170726](https://github.com/lm3nitro/Projects/assets/55665256/8aa072b7-f8a2-40d8-bcca-a06957e32d8e)

![Pasted image 20240505170747](https://github.com/lm3nitro/Projects/assets/55665256/14a43935-2df2-4620-820e-449287067a67)


![Pasted image 20240505170813](https://github.com/lm3nitro/Projects/assets/55665256/5ec5349b-b54e-4d16-9892-18651c7bf206)

![Pasted image 20240505170839](https://github.com/lm3nitro/Projects/assets/55665256/bb1b8026-e1bc-441e-bfba-025a9dfcf0da)


![Pasted image 20240505170906](https://github.com/lm3nitro/Projects/assets/55665256/2a0ede17-4cc8-4db5-8b22-0208869051d7)

![Pasted image 20240505170933](https://github.com/lm3nitro/Projects/assets/55665256/47bce11b-95b8-4922-9247-163230086e65)


![Pasted image 20240505170955](https://github.com/lm3nitro/Projects/assets/55665256/2a565ada-a63e-48e3-a251-f3af275e1e45)


![Pasted image 20240505171142](https://github.com/lm3nitro/Projects/assets/55665256/7ff95fea-f927-4184-85fe-550b2c274497)


![Pasted image 20240505171335](https://github.com/lm3nitro/Projects/assets/55665256/7f12ac10-7c15-4cc7-b8af-34fccf777e18)


![Pasted image 20240505171408](https://github.com/lm3nitro/Projects/assets/55665256/6e95892c-e215-4a88-9c37-7424179e8b1c)

![Pasted image 20240505171433](https://github.com/lm3nitro/Projects/assets/55665256/02c44957-ed0c-44a2-95b8-bdfe05d9cd48)

![Pasted image 20240505171503](https://github.com/lm3nitro/Projects/assets/55665256/70952cc9-bd3e-4ab5-976d-6240720b8623)


![Pasted image 20240505171522](https://github.com/lm3nitro/Projects/assets/55665256/02e9c72e-4173-4171-abe9-fbb800a8289f)

DoS attack to WINDOWS 10


![Pasted image 20240505173923](https://github.com/lm3nitro/Projects/assets/55665256/20774dfe-0fbf-4212-bce7-792c0c49222e)


![Pasted image 20240505173859](https://github.com/lm3nitro/Projects/assets/55665256/bc21365c-a951-42f6-8a17-067060a1c70e)


![Pasted image 20240505174217](https://github.com/lm3nitro/Projects/assets/55665256/cf774d4a-4cea-4e85-b649-1a4b2a512994)


POD attack


![Pasted image 20240505175619](https://github.com/lm3nitro/Projects/assets/55665256/40c232e9-f3df-4478-b544-ff4923067f01)


![Pasted image 20240505175527](https://github.com/lm3nitro/Projects/assets/55665256/9344f369-bd9b-47a4-bcd9-c41e7323c157)


More details


![Pasted image 20240505180225](https://github.com/lm3nitro/Projects/assets/55665256/633a290f-5cfa-4733-ac44-303738b62ec0)


Much better:
![Pasted image 20240505175809](https://github.com/lm3nitro/Projects/assets/55665256/ec12c831-57d8-4839-a27a-7f5f741f3036)

### Summary:

This exercise covers attack detection, traffic analysis, and mitigation techniques, ultimately enhancing our understanding of DDoS vulnerabilities and defenses in network security.

