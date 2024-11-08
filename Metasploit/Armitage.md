# Armitage

Armitage is a graphical user interface (GUI) for the Metasploit Framework, which a popular tool used in cybersecurity for penetration testing. These are just a few of the features that it provides:

+ Visual Interface
+ Network Discovery
+ Session Management
+ Exploit Management
+ Reporting
+ Attack Automation
+ Team Collaboration

![Pasted image 20240425133244](https://github.com/lm3nitro/Projects/assets/55665256/5743bbb1-eca6-4f43-ac87-2f38152a18ae)

### Scope:

I will be installing Armitage on a Linux OS host and will be performing a scan against the target host to identify any vulnerabilities present, exploiting them, and then analyzing the traffic and logs generated from the attack. 

### Tools and Technology:
Armitage, Linux OS, and Wireshark

## Installation:

To get started I began by installing Armitage using the command below:

```
apt install armitage
```

![Pasted image 20240425133657](https://github.com/lm3nitro/Projects/assets/55665256/c1fac9ca-73e7-40c9-bd73-29a9c2f99b24)

## Starting up Armitage:

I used the following command to get started

```
msfdb init
```
This creates and begins execution of the database and the web service

![Pasted image 20240425133908](https://github.com/lm3nitro/Projects/assets/55665256/68fcb7a0-f140-412e-ab24-8ef084573213)

I started the service:

![Pasted image 20240425133921](https://github.com/lm3nitro/Projects/assets/55665256/fb446cb7-4e48-42f6-a65d-a37b28680e03)

Running Armitage:

```
armitage
```

I was then prompted to enter the IP, port, user and password:

![Pasted image 20240425134621](https://github.com/lm3nitro/Projects/assets/55665256/9af7130e-c33b-40c9-9ee9-5d0349869f2d)

Connecting to the database:

![Pasted image 20240425134648](https://github.com/lm3nitro/Projects/assets/55665256/582c89a9-d0d8-47d3-a547-6d992db990e1)

Once connected, I was given the user interface:

![Pasted image 20240425134854](https://github.com/lm3nitro/Projects/assets/55665256/266c4b1e-4fe8-429f-a657-9302acbdc446)

## Creating a Target

I went to Hosts > Add Hosts:

![Pasted image 20240425134958](https://github.com/lm3nitro/Projects/assets/55665256/3ae3d398-e944-4b2d-a519-2cc8a67ff082)

Entered the IP address of the host I wanted to target:

![Pasted image 20240425135029](https://github.com/lm3nitro/Projects/assets/55665256/48ea933b-7057-4c39-b38c-d1e70bc9f330)

## Reconnaissance

To begin I wanted to perform a Nmap scan against the target. I went to Hosts > Nmap Scan > Quick Scan (OS detect)

![Pasted image 20240425135614](https://github.com/lm3nitro/Projects/assets/55665256/91ad237f-83dd-4d34-b4f5-e78d6e4b21aa)

After choosing the type of scan, I could see the full command that is being ran against the host in the command line window below. The Nmap scan provides us with the list of all the ports that are open on the hosts and versions of services that are running:

![Pasted image 20240425135540](https://github.com/lm3nitro/Projects/assets/55665256/8c656b30-52e6-45c2-9f27-fb99cf007092)

## Launching Exploit

Based on the Nmap scan above, the host has the csftpd port open and running version 2.3.4. Armitage has a backdoor exploit for this specific version:

![Pasted image 20240425135750](https://github.com/lm3nitro/Projects/assets/55665256/fcc35618-d827-4e2d-ba48-1f69270cdb28)

These are the details of the exploit. I then proceeded to launch the attack:

![Pasted image 20240425140017](https://github.com/lm3nitro/Projects/assets/55665256/c28d8173-afd6-4b73-8e76-5ae65012ba8e)

Once the attack is successful, we can see that the image of the computer pc changes to show that it has been compromised. 

![Pasted image 20240425140151](https://github.com/lm3nitro/Projects/assets/55665256/33634aa2-40e3-4e4d-bdf4-39d75346a67a)

The console view at the bottom also shows that the attack has been successful. It also provides information on the IP and port where the connection was made:

![Pasted image 20240425140128](https://github.com/lm3nitro/Projects/assets/55665256/64353001-5d0f-4558-8895-83127ad4b5fb)

## Analysis

I ran Wireshark while performing the attack as well so that I could see what this attack would look like in the network traffic. Here we can see the source and destination IP and port along with the information on the attack. It looks like the attack was able to give the adversary root privileges:

![Pasted image 20240425140600](https://github.com/lm3nitro/Projects/assets/55665256/79f8bcf9-7ac4-4d51-923f-d475bc6deb2f)

I then went to the target host and was also able to see the connection logs:

![Pasted image 20240425141001](https://github.com/lm3nitro/Projects/assets/55665256/f4229242-6fa9-47c9-82cc-a97c8bb59165)

Analyzing the FTP service logs we can see that the authentication was successful:

![Pasted image 20240425141756](https://github.com/lm3nitro/Projects/assets/55665256/c783e3a8-aaee-4870-9b47-ebfde8df4f0c)

After launching the attack and creating the backdoor, I also wanted to explore some of the features that Armitage has. In this case, because I had created the backdoor and was connected to the host, I decided to open a shell command and type in some inputs. To do this, I right-clicked on the host, selected Shell1 > Interact:

![Pasted image 20240425142047](https://github.com/lm3nitro/Projects/assets/55665256/0d06704c-ab3b-483c-b7f5-f7b8e8f6baee)

These are the inputs provided:

![Pasted image 20240425142358](https://github.com/lm3nitro/Projects/assets/55665256/5e7758e2-8960-46fc-9f4b-2530a2b7ef5b)

Looking at Wireshark, which is still running, I can see in clear text everything that was being done:

![Pasted image 20240425142940](https://github.com/lm3nitro/Projects/assets/55665256/4481bf98-2e84-4ada-bbec-fe608631e386)

### Summary and Analysis

Initially, I performed a network discovery scan to identify hosts, running services and open ports. I then identified that vsftpd was open and was running a vulnerable version 2.3.4. I then used this to exploit the vulnerability and used a backdoor provided by Armitage. I then analyzed the attack from the victim perspective in both the ftp logs, connection logs, and Wireshark. 

I was able to perform network discovery, vulnerability identification, exploit deployment, and session management. This allowed me to apply theoretical knowledge and turn it into hands on experience. Also, the visual interface helped to see the attack progress clearly. Armitage allowed me to enhance understanding and efficiency, and I can definitely see how this is a valuable tool for learning and getting practical experience.  
