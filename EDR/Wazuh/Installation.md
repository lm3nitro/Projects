# Wazuh

![Pasted image 20240428133059](https://github.com/lm3nitro/Projects/assets/55665256/2b36c7a2-1c9d-480a-a9d5-0c51620aa2fb)

Wazuh is a free and open source security monitoring platform. Wazuh collects, normalizes, and analyzes log data from various sources such as Operating System, applications, and network devices. It provides real-time correlation, context, and response for security events and incidents, as well as cloud environments and management.

Listed below are some core functionalities of Wazuh: 

+ Intrusion Detection
+ Log Management
+ File Integrity Monitoring
+ Vulnerability Detection
+ Security Compliance

### Objective:

Install and configure Wazuh in a test environment and evaluate its effectiveness in detecting and responding to security threats. 

### Scope:


### Tools ans Technology:



# Installation:

![Pasted image 20240428142441](https://github.com/lm3nitro/Projects/assets/55665256/6d370510-b525-4e1b-a218-6bef5eaa44bf)


# Checking the status of the Wazuh server:

systemctl status wazuh-manager

![Pasted image 20240428140610](https://github.com/lm3nitro/Projects/assets/55665256/442e9145-2801-424f-9684-41f6b117a26e)




#  Restart wazuh manager server:


systemctl restart wazuh-manager

# Looking for the IP of the Wazuh server:

![Pasted image 20240428133459](https://github.com/lm3nitro/Projects/assets/55665256/4f0e3afa-2d74-4fac-b530-d3eab7f5514d)

# Login page:

![Pasted image 20240428133706](https://github.com/lm3nitro/Projects/assets/55665256/2dda9435-acb1-430d-8165-04b5a26d2628)

# Adding Agent:

![Pasted image 20240428134105](https://github.com/lm3nitro/Projects/assets/55665256/7f22eccd-e4c5-43e0-84f8-b9494fc2dfe0)

![Pasted image 20240428134352](https://github.com/lm3nitro/Projects/assets/55665256/22fb45c4-11c3-4cd0-83ec-e93137883e92)

![Pasted image 20240428134509](https://github.com/lm3nitro/Projects/assets/55665256/3a0608f2-7f9a-4c45-84bb-16f89635e78d)


Pasting the command on Powershell

![Pasted image 20240428134550](https://github.com/lm3nitro/Projects/assets/55665256/11383642-9b2e-45d4-a447-4e82e5383fd1)


Downloading Agent:

![Pasted image 20240428134628](https://github.com/lm3nitro/Projects/assets/55665256/9ecdbf7a-fab4-4486-9ead-5cb88914114c)

# Starting the service:

![Pasted image 20240428134733](https://github.com/lm3nitro/Projects/assets/55665256/87ba671b-b575-417a-a23e-e4e8c0d4b2fb)


# Restart the service:

Restart-Service wazuh 

# Checking the service:


![Pasted image 20240428135323](https://github.com/lm3nitro/Projects/assets/55665256/1d7a8b06-6e2f-4f87-9691-240b7c1b68a5)



# Network connection :

![Pasted image 20240428135824](https://github.com/lm3nitro/Projects/assets/55665256/898ab9bc-3e5b-4f39-91cd-fb3bf89ca56b)

![Pasted image 20240428140327](https://github.com/lm3nitro/Projects/assets/55665256/442d1863-ff92-44ae-8d83-6bdc4b4be00a)



# Login Back to Wazuh:


![Pasted image 20240428135034](https://github.com/lm3nitro/Projects/assets/55665256/fa78f0b2-b29b-43fa-b679-105e848bc9bb)

![Pasted image 20240428135201](https://github.com/lm3nitro/Projects/assets/55665256/f7638f52-9ac0-433c-abb0-aef4ce5efe2a)


# Security Events:

![Pasted image 20240428141534](https://github.com/lm3nitro/Projects/assets/55665256/11fee0ee-8a86-4977-8d9c-df4e5cbb4a93)




# Adding Linux Agent:


![Pasted image 20240428155628](https://github.com/lm3nitro/Projects/assets/55665256/60b6275f-d87d-498a-8012-e2554533db75)




Downloading the agent:



![Pasted image 20240428155752](https://github.com/lm3nitro/Projects/assets/55665256/34449676-8166-4739-bf2b-4e894eb4a51c)



wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.3-1_amd64.deb && sudo WAZUH_MANAGER='10.10.100.47' WAZUH_AGENT_NAME='sql_lm3nitro_database' dpkg -i ./wazuh-agent_4.7.3-1_amd64.deb




# Enable agent:


sudo systemctl start wazuh-agent
sudo systemctl enable wazuh-agent
sudo systemctl daemon-reload


![Pasted image 20240428155913](https://github.com/lm3nitro/Projects/assets/55665256/fa96ec13-37a4-4d64-b2e7-5d72757f4df1)



# Verify connectivity:

![Pasted image 20240428160241](https://github.com/lm3nitro/Projects/assets/55665256/33e5eaaa-6790-4a0d-9d8a-b416416520a6)


![Pasted image 20240428160441](https://github.com/lm3nitro/Projects/assets/55665256/2e4c7fac-cb35-4875-b678-e9e1444339d5)



# Looking at the Wazuh agent:

![Pasted image 20240428160544](https://github.com/lm3nitro/Projects/assets/55665256/b6106dc5-49fa-40ff-9d53-b41a4147a2e5)


# Security Events:


![Pasted image 20240428160701](https://github.com/lm3nitro/Projects/assets/55665256/b4afb69f-5eff-4de7-b0c2-847ff2dc42f3)



# Raw logs:

![Pasted image 20240428161502](https://github.com/lm3nitro/Projects/assets/55665256/a1fb98e0-4eb0-437c-b1f2-3ea66fc2a4c4)


========================




# activating vulnerability module

text files that allows s to tweak wazuh

navigating to /var/ossec/ 

![Pasted image 20240428162723](https://github.com/lm3nitro/Projects/assets/55665256/56e4cb7b-8f30-472a-8ed5-284010d3eb99)

![Pasted image 20240428163614](https://github.com/lm3nitro/Projects/assets/55665256/9013a186-9fa9-4ab3-9bde-17ba234cf29c)

![Pasted image 20240428163608](https://github.com/lm3nitro/Projects/assets/55665256/32b37077-6198-4855-a79f-a49f91084802)


Restarting Wazuh manager:

![Pasted image 20240428163922](https://github.com/lm3nitro/Projects/assets/55665256/ee09ff7b-f199-4e5b-9fdd-9991344b2602)

Restarting Agents:

Note:

 It needs admin privileges to restart the services both on linux and windows.


Ubuntu 22.04

![Pasted image 20240428164237](https://github.com/lm3nitro/Projects/assets/55665256/44234f6d-5d71-49b0-b82c-19b6c7a80f1e)



Windows 10
![Pasted image 20240428164428](https://github.com/lm3nitro/Projects/assets/55665256/4b5258a1-8f51-43b2-8e27-cf2c5ada3bca)




