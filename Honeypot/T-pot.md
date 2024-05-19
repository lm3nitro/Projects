
![Pasted image 20240512155316](https://github.com/lm3nitro/Projects/assets/55665256/905c1e26-7020-4389-a3c4-7a483917bb2f)



T-POT is **an open-source honeypot framework designed to simplify the deployment and management of honeypots for capturing and analyzing cyber threats**. It offers easy deployment, supports multiple honeypot technologies, provides security and monitoring features, and allows centralized management.


In this scenario, we will installing a honeypot on the internal network. Honeypots are usually deployed **inside production networks alongside production servers**; the honeypot acts as a decoy, drawing intruders away from the production network as part of the intrusion detection system (IDS).


Network Diagram:

![Pasted image 20240513170453](https://github.com/lm3nitro/Projects/assets/55665256/3d132f4d-ef4d-45d5-96c4-7ec154011d9c)


Installing:



git clone https://github.com/telekom-security/tpotce.git


![Pasted image 20240512144341](https://github.com/lm3nitro/Projects/assets/55665256/72e404b9-debe-4edb-a17c-56a29c7f697a)



Verify the IP address of the server

![Pasted image 20240512144403](https://github.com/lm3nitro/Projects/assets/55665256/2a6f43f2-d190-4258-b933-36c71b6b4835)


Listing the file downloaded

![Pasted image 20240512144446](https://github.com/lm3nitro/Projects/assets/55665256/2417e464-b6d6-4f8d-9ad0-349e43016c06)


Change to tpotce  directory and locate the install.sh script and execute it with non root privilege 


![Pasted image 20240512144513](https://github.com/lm3nitro/Projects/assets/55665256/c5e193f7-2ce2-4a18-9306-cfbebee4960d)



Select (H)ive

![Pasted image 20240512144542](https://github.com/lm3nitro/Projects/assets/55665256/b087cfc5-c97d-48ea-9839-abec3b160d9d)


![Pasted image 20240512144615](https://github.com/lm3nitro/Projects/assets/55665256/7438d262-00eb-4fea-8f32-023326f84cee)


![Pasted image 20240512144641](https://github.com/lm3nitro/Projects/assets/55665256/78e28dab-d882-4e50-947d-b9ba660f6d21)

sudo reboot



Once the T-Pot Installer successfully finishes, the system needs to be rebooted (`sudo reboot`). Once rebooted you can log into the system using the user you setup during the installation of the system.


Login from your browser and access the T-Pot WebUI and tools: `https://<your.ip>:64297`




![Pasted image 20240512145510](https://github.com/lm3nitro/Projects/assets/55665256/375f3767-335d-47a6-8d5d-7724c493f309)



Runnnig a scan to generate some data:

![Pasted image 20240512153853](https://github.com/lm3nitro/Projects/assets/55665256/62043537-55d6-421b-84c9-25e40885e827)


As you can see the the honeypot seems to have all of these ports opened

![Pasted image 20240512153744](https://github.com/lm3nitro/Projects/assets/55665256/46334f59-7e40-40fc-9875-0364fa7031b4)


Informatimation from the scan: 

Selecting Kibana on the main Tpot main page:

![Pasted image 20240512155012](https://github.com/lm3nitro/Projects/assets/55665256/646b4964-91ca-42d8-80c8-d1cecfd268ba)



Search for Tpot dashboard:



![Pasted image 20240512154827](https://github.com/lm3nitro/Projects/assets/55665256/352f7398-d83d-424c-b8e1-c879a0edb35b)



Looking at the attacks:


![Pasted image 20240512151418](https://github.com/lm3nitro/Projects/assets/55665256/93804c50-4f0c-4efe-9690-3e1ab86cf61d)


Attacker IP:


![Pasted image 20240512151626](https://github.com/lm3nitro/Projects/assets/55665256/fddf9833-6433-49ca-9dd5-014df59b187c)



 Raw data from the attacker IP:
 
![Pasted image 20240512152156](https://github.com/lm3nitro/Projects/assets/55665256/42bcc2a3-3c7d-4113-8ce2-e170099e7274)





summary:


