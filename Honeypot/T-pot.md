# T-Pot

![Pasted image 20240512155316](https://github.com/lm3nitro/Projects/assets/55665256/905c1e26-7020-4389-a3c4-7a483917bb2f)

T-Pot is an open-source honeypot framework designed to simplify the deployment and management of honeypots for capturing and analyzing cyber threats. It integrates several honeypot technologies, tools, and services into a sing, comprehensive system to attract, detect, and analyze cyber threats.

### Scope: 
I will be installing T-Pot on a server in my internal network. I will then use another host on the network to simulate a scan and identify open ports and services on T-Pot. The adversary will often scan their targets to gather information on potential attack vectors and weak points to gain authorized access or disrupt services. Once complete, I will look at the events collected from the scan in T-Pot. 

>#### Note: Honeypots are usually deployed inside production networks alongside production servers; the honeypot acts as a decoy, drawing intruders away from the production network as part of the intrusion detection system (IDS). 

### Tools and Technology:

Linux OS, Nmap, and T-Pot

This is a look at the network diagram:

![Pasted image 20240513170453](https://github.com/lm3nitro/Projects/assets/55665256/3d132f4d-ef4d-45d5-96c4-7ec154011d9c)


## Installing T-Pot:
To get started, I used the git clone command below. This will allow me to download the entire repository, branches, files, etc, for T-Pot. 
```
git clone https://github.com/telekom-security/tpotce.git
```

![Pasted image 20240512144341](https://github.com/lm3nitro/Projects/assets/55665256/72e404b9-debe-4edb-a17c-56a29c7f697a)

I also verified the IP address of the server:

![Pasted image 20240512144403](https://github.com/lm3nitro/Projects/assets/55665256/2a6f43f2-d190-4258-b933-36c71b6b4835)


I was able to see that the file was downloaded:

![Pasted image 20240512144446](https://github.com/lm3nitro/Projects/assets/55665256/2417e464-b6d6-4f8d-9ad0-349e43016c06)


I then changed into the tpotce directory and located the install.sh script and executed it with non-root privilege.

> [!NOTE]  
> It is important to run the script with a non-root user because it minimizes the potential damage an attacker can cause is they manage to compromise the honeypot. This will help in containing the impact and preventing the attacker from gaining full control of the system.

![Pasted image 20240512144513](https://github.com/lm3nitro/Projects/assets/55665256/c5e193f7-2ce2-4a18-9306-cfbebee4960d)

Once the I executed the script, I was given the option to choose the type of install, I chose *(H)ive*. I was also prompted to enter a username and password:

![Pasted image 20240512144542](https://github.com/lm3nitro/Projects/assets/55665256/b087cfc5-c97d-48ea-9839-abec3b160d9d)

After, I was able to see all the pulling being done:

![Pasted image 20240512144615](https://github.com/lm3nitro/Projects/assets/55665256/7438d262-00eb-4fea-8f32-023326f84cee)

When the T-Pot Installer successfully finished, I then had to reboot the system. I was prompted to login via SSH on port 64295. 

![Pasted image 20240512144641](https://github.com/lm3nitro/Projects/assets/55665256/78e28dab-d882-4e50-947d-b9ba660f6d21)
```
sudo reboot
```

After the system had rebooted, I was able to login from my browser and access the T-Pot WebUI and tools: 
```
https://<your.ip>:64297
```

![Pasted image 20240512145510](https://github.com/lm3nitro/Projects/assets/55665256/375f3767-335d-47a6-8d5d-7724c493f309)


## Scan Emulation
Runnnig a scan to generate some data:

![Pasted image 20240512153853](https://github.com/lm3nitro/Projects/assets/55665256/62043537-55d6-421b-84c9-25e40885e827)

As you can see the honeypot seems to have all of these ports opened:

![Pasted image 20240512153744](https://github.com/lm3nitro/Projects/assets/55665256/46334f59-7e40-40fc-9875-0364fa7031b4)

## Scan Analysis

After performing the scan against the host, I logged into T-Pot to see what information it was able to gather about the scan. 

To get started, I selected Kibana from the T-Pot main page:

![Pasted image 20240512155012](https://github.com/lm3nitro/Projects/assets/55665256/646b4964-91ca-42d8-80c8-d1cecfd268ba)

T-Pot comes with a series of dashboards that are pre-built. From the menu, I searched for the T-pot dashboard:

![Pasted image 20240512154827](https://github.com/lm3nitro/Projects/assets/55665256/352f7398-d83d-424c-b8e1-c879a0edb35b)

Here we can see details about the simulated attack/scan. This dashboard allows us to see the number of attacks detected, its origin, usernames targets, OS from the attacker, etc.:

![Pasted image 20240512151418](https://github.com/lm3nitro/Projects/assets/55665256/93804c50-4f0c-4efe-9690-3e1ab86cf61d)

T-Pot also allows us to take a closer look. Here I was able to see the IP address of the attacks, CVE information, and the Suricata alert signatures that were matched by the attack:

![Pasted image 20240512151626](https://github.com/lm3nitro/Projects/assets/55665256/fddf9833-6433-49ca-9dd5-014df59b187c)

This is another look into the raw data from the attacker IP:
 
![Pasted image 20240512152156](https://github.com/lm3nitro/Projects/assets/55665256/42bcc2a3-3c7d-4113-8ce2-e170099e7274)


### Summary:

Having a host out in the wild that is public facing is very risky. T-Pot allows you to be able to detect and analyze numerous threats by simulating various services and systems. Having T-Pot also allows you to gain insight and information into the techniques used by actors. Having the tactics being used allows us to better protect ourselves, reduce the impact of an attack, and fine tune our current Security to bolster the cybersecurity posture. T-pot can also be used to identify weak spots and vulnerabilities. 

