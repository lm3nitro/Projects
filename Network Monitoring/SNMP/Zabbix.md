# Zabbix:

![Pasted image 20240515164306](https://github.com/lm3nitro/Projects/assets/55665256/e87e6653-8c3e-4c87-929a-e23fd78f601c)

Zabbix is an open source monitoring tool designed to track the health of your IT infrastructure. Zabbix collects and analyzes data from network devices, servers, and applications and provides real-time insights and alerts. It supports a wide range of monitoring methods, including SNMP, agent-based monitoring, and web monitoring. 

Features:
+ Scalability
+ Data collection and storage
+ Real-time monitoring
+ Customizable dashboards

### Scope:


### Tools and Technology:


## Installation

Main page:

![Pasted image 20240515164420](https://github.com/lm3nitro/Projects/assets/55665256/7de60618-caf5-44a3-bcfb-f52581d018fa)



downloading:


![Pasted image 20240515163648](https://github.com/lm3nitro/Projects/assets/55665256/56bf527d-dd53-451d-a523-d16db0dd7f1d)


Untar the file:


![Pasted image 20240515164107](https://github.com/lm3nitro/Projects/assets/55665256/577778b4-727b-409b-a474-701c850cfb25)


Importing the appliance to VMware:


![Pasted image 20240515164607](https://github.com/lm3nitro/Projects/assets/55665256/32f308cb-a2d3-4241-bb42-e4d33603884e)

Editing the Vm and adding the resources needed: 



![Pasted image 20240515164743](https://github.com/lm3nitro/Projects/assets/55665256/4d809fc7-7a1b-4561-a2d4-f7432dee32ab)




Network Diagram: 




![Pasted image 20240519155020](https://github.com/lm3nitro/Projects/assets/55665256/6bc83c4a-6735-4a8a-905c-289c6c35e694)


Console Credentials :

System:

User: root
Password: zabbix

![Pasted image 20240515165032](https://github.com/lm3nitro/Projects/assets/55665256/ceb90c66-a255-427c-8b5b-c466bfb4abb7)



login in via SSH:

![Pasted image 20240515165413](https://github.com/lm3nitro/Projects/assets/55665256/b3f31524-6515-4045-8716-bdd16ddfef32)




Login in WebUI:


User: Admin
Password: zabbix

![Pasted image 20240515165313](https://github.com/lm3nitro/Projects/assets/55665256/e8236635-adb9-4187-b458-5b7e46f88fa4)


Main Dashboard:


![Pasted image 20240515165627](https://github.com/lm3nitro/Projects/assets/55665256/52be766c-7ac6-4cd7-9d96-9ce0585d15f5)




Installing a Zabbix agent in Pfsense:




![Pasted image 20240515220813](https://github.com/lm3nitro/Projects/assets/55665256/821fd723-02ac-4f63-8d80-86d8cf9a93eb)


![Pasted image 20240515220743](https://github.com/lm3nitro/Projects/assets/55665256/5c5cfb87-6e47-43f7-af6a-16e0a42d3158)




Installation completed:

![Pasted image 20240515220848](https://github.com/lm3nitro/Projects/assets/55665256/7186fa20-5058-41ed-aac9-2092e57a1208)




Configuring Zabbix agent:

![Pasted image 20240515221022](https://github.com/lm3nitro/Projects/assets/55665256/044abfeb-33ce-40ff-bce7-da5615264f59)





![Pasted image 20240515221059](https://github.com/lm3nitro/Projects/assets/55665256/5f8f1022-3ace-4a5b-8071-ceee53c2fe7f)






Creating a custom rule for the Zabbix server to pull data from the Pfsense firewall:

![Pasted image 20240515221154](https://github.com/lm3nitro/Projects/assets/55665256/d79424dc-1db8-4d5d-97fe-d06ec96f5569)

Creating the rule:

![Pasted image 20240515221221](https://github.com/lm3nitro/Projects/assets/55665256/e81d70c5-e208-44b4-ae80-b78aee970013)



reviewing the rule:

![Pasted image 20240515221257](https://github.com/lm3nitro/Projects/assets/55665256/02b428dc-277c-447a-9514-e0e947d4695f)




Adding Pfsense firewall:

![Pasted image 20240515214938](https://github.com/lm3nitro/Projects/assets/55665256/b87d1773-fcf9-4511-ac45-8ec10a26c2c5)



![Pasted image 20240515215235](https://github.com/lm3nitro/Projects/assets/55665256/5e85c6c8-d30b-49a0-8c65-9be5bef1758a)



Let's look at the firewall logs:

![Pasted image 20240515221525](https://github.com/lm3nitro/Projects/assets/55665256/bb1e103c-a747-47d6-bb34-1603d0774369)


Taking a packet caputure to analsys the traffic:

![Pasted image 20240515221840](https://github.com/lm3nitro/Projects/assets/55665256/90b8975c-6b10-439e-8f33-47366bfc674a)



Traffic information about the communication:

![Pasted image 20240515222257](https://github.com/lm3nitro/Projects/assets/55665256/dfac540b-d5d4-422f-948e-f35bbae1a146)


Let's monitor our Pfsense firewall:


![Pasted image 20240515220000](https://github.com/lm3nitro/Projects/assets/55665256/01810ed9-9a02-4405-9a40-21197e751cc2)



All the graphs listed:

![Pasted image 20240515220042](https://github.com/lm3nitro/Projects/assets/55665256/b26b24d2-b2ae-4adf-bc25-f5479d1d7969)




Graphs configuration tab:

![Pasted image 20240515220151](https://github.com/lm3nitro/Projects/assets/55665256/2a9c9cd6-bd39-43c5-93ec-562bb07b4b88)



Going back and selecting a Dashboard of the Pfsense firewall:

![Pasted image 20240515215749](https://github.com/lm3nitro/Projects/assets/55665256/4a5cbfa7-45ce-4389-a2e0-c581e71928c7)


![Pasted image 20240515215837](https://github.com/lm3nitro/Projects/assets/55665256/ee3b11f2-952b-465b-9628-65769f0ca54e)







