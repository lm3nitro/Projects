# Security Onion

<img width="504" alt="Screenshot 2024-06-04 at 9 45 41 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/4d38d1d3-f457-4f9c-9303-4b7303d10de2">

Security Onion is a free and open-source Linux distribution for intrusion detection, network security monitoring, and log management. It combines various powerful tools such as Zeek, Suricata, and the Elastic Stack, all into a single platform. It also offers other features:

+ Pre-configured dashboards
+ Automated data collection
+ Streamlined analysis workflows

### Scope: 

I will be installing the Security Onion ISO in VMWare workstation. This will be connected to the SPAN port of the switch that is mirroring all the traffic coming and going to and from the network. I purposefully have a Metasploitable VM in my network. I will perform a scan against the targeted Metaspolitable VM and will be analyzing the scan in Security Onion. Below is a view at the architecture: 

![Pasted image 20240513165051](https://github.com/lm3nitro/Projects/assets/55665256/965552da-376a-447c-ac66-59a9b4a57e03)

### Tools and Technology:

Linux OS, Security Onion, VMWare Workstation, InfluxDB, Elastic, and Metasploitable
## Installation

To begin, I navigated to the main website and went to *Download*

https://securityonionsolutions.com/software

![Pasted image 20240513113542](https://github.com/lm3nitro/Projects/assets/55665256/499fb80b-ebb5-4de9-8c7d-ad15eb82652c)

Downloaded the ISO:

![Pasted image 20240513113729](https://github.com/lm3nitro/Projects/assets/55665256/6fb23544-e3f9-4736-9869-bd5e3d4f9f9b)

Once downloaded, I went to VMWare Workstation and selected *Create a New Virtual Machine* and selected the newly downloaded ISO:

![Pasted image 20240513112325](https://github.com/lm3nitro/Projects/assets/55665256/b8b662b6-ee28-4183-b3c7-5271762c6373)

After the ISO is selected, a new diagram box comes up to configure the VM. The first step is choosing a name:

![Pasted image 20240513112536](https://github.com/lm3nitro/Projects/assets/55665256/77b0f708-4d23-41d8-95ec-ab2774c364d3)

Then was prompted to select the virtual drive size:

![Pasted image 20240513112617](https://github.com/lm3nitro/Projects/assets/55665256/1c4c4e10-58b5-43f2-a2e5-666b79e7d8cf)

For this VM I chose to create custom hardware with the following:

+ 16 virtual cores
+ 32 GB of ram
+ 2 NICs card (One of the network cards for administration and the other one for sniffing)

![Pasted image 20240513112852](https://github.com/lm3nitro/Projects/assets/55665256/f0c7f6f7-e02c-4415-bf54-5673187c576f)

Now that the VM specs have been selected, it's time to turn on the VM:

![Pasted image 20240513113012](https://github.com/lm3nitro/Projects/assets/55665256/71cd5e03-ca3c-4a01-a62d-9a484c023a76)

When the VM first boots up I was presented with a warning. I selected *yes* to proceed

![Pasted image 20240513113840](https://github.com/lm3nitro/Projects/assets/55665256/99b94cb9-a841-4e9f-bbfb-4e5958b5e0b7)

I was then prompted to create a username/password. Once entered the installation begins:

![Pasted image 20240513114033](https://github.com/lm3nitro/Projects/assets/55665256/ed1b8bef-a43b-4ea2-be0d-8120a61e8e35)

After the installation completes, reboot the VM. After rebooting, I logged in:

![Pasted image 20240513120008](https://github.com/lm3nitro/Projects/assets/55665256/7341a588-51ee-4f27-84b5-347c562363fb)

## Security Onion Setup 
Upon logging in the installation set up will begin:

![Pasted image 20240513120133](https://github.com/lm3nitro/Projects/assets/55665256/9e23ddd2-66b5-48a3-9ed3-c323d168f494)

Proceeded with the standard Security Onion installation:

![Pasted image 20240513120201](https://github.com/lm3nitro/Projects/assets/55665256/0c477838-97a8-41c6-9369-490d8d0b6b7e)

Chose the standalone option: 

![Pasted image 20240513120242](https://github.com/lm3nitro/Projects/assets/55665256/9de0b989-0665-412f-89e0-68597bc0f2b6)

Agreed to the terms:

![Pasted image 20240513120320](https://github.com/lm3nitro/Projects/assets/55665256/edea39d4-0903-490f-8820-6ea4bc9a355b)

![Pasted image 20240513120343](https://github.com/lm3nitro/Projects/assets/55665256/03756069-a5e9-488a-baf1-47bf2490d880)

![Pasted image 20240513120522](https://github.com/lm3nitro/Projects/assets/55665256/589db176-840f-4720-b451-a9144885c64c)

![Pasted image 20240513120624](https://github.com/lm3nitro/Projects/assets/55665256/ffcd4057-9200-412b-b596-37cf019c5240)

Selected the management interface:

![Pasted image 20240513121208](https://github.com/lm3nitro/Projects/assets/55665256/7370130e-52eb-45ad-831c-2fdc3731b3f6)

Network configuration:

![Pasted image 20240513121250](https://github.com/lm3nitro/Projects/assets/55665256/5cff7c3b-175d-48d7-b2d9-8562aa72c1b8)

![Pasted image 20240513121335](https://github.com/lm3nitro/Projects/assets/55665256/36a6d75c-9154-4d70-b283-521a5af40333)

![Pasted image 20240513121420](https://github.com/lm3nitro/Projects/assets/55665256/8bf82766-98d2-49cc-a9f1-4173dfd74ce7)

![Pasted image 20240513121446](https://github.com/lm3nitro/Projects/assets/55665256/b95e621d-b8bf-4d1e-9e2c-7bcfabd67f82)

![Pasted image 20240513121510](https://github.com/lm3nitro/Projects/assets/55665256/6bd10ccb-e389-4798-986c-4bd41f2694cd)

My connection will be direct as I will not be using a proxy:

![Pasted image 20240513121536](https://github.com/lm3nitro/Projects/assets/55665256/f44c34b7-3f48-4c96-b367-50c678e6073b)

![Pasted image 20240513121558](https://github.com/lm3nitro/Projects/assets/55665256/a1231246-0389-4eea-ae91-39d6b2f72e2f)

Selected the sniffing interface:

![Pasted image 20240513121640](https://github.com/lm3nitro/Projects/assets/55665256/0c56dc5e-521d-4365-8b9b-ac6bd9b374c2)

When selecting the email, it can be any email address:

![Pasted image 20240513121735](https://github.com/lm3nitro/Projects/assets/55665256/afa4ecff-3b25-43f7-92fd-116e4b476ef9)

![Pasted image 20240513121756](https://github.com/lm3nitro/Projects/assets/55665256/2a704d3c-1ffa-4937-840f-26b301f39a7e)

I decided to go with using the IP address, in my case it is the 10.10.100.70:

![Pasted image 20240513121832](https://github.com/lm3nitro/Projects/assets/55665256/140c3bfd-1883-4134-9381-5c9a405afd94)

![Pasted image 20240513121859](https://github.com/lm3nitro/Projects/assets/55665256/b75e2bc5-f6ee-4508-8560-285fb4ee2674)

![Pasted image 20240513121944](https://github.com/lm3nitro/Projects/assets/55665256/6e609108-5855-4a72-b6c8-95bd6c31141f)

After making all the selections, you are presented with a summary of all the options you have chosen and the input from above. After reviewing, selected *Yes* to proceed:

![Pasted image 20240513122011](https://github.com/lm3nitro/Projects/assets/55665256/ce8cd339-c7e4-4bce-8fa9-56f3fc7944f1)

Once the installation has finished, you will be prompted with the information down below for access:

![Pasted image 20240513132823](https://github.com/lm3nitro/Projects/assets/55665256/4b7ee7b9-4b16-449b-b47b-26b21bba4c07)

## Accessing Security Onion Web Interface

As previously stated, I chose to access the web interface with the IP. Here I used the same email and password that I entered during the setup:

![Pasted image 20240513133158](https://github.com/lm3nitro/Projects/assets/55665256/988a99ad-fcb5-4fa9-a839-f859bd6b245d)

I first looked at the dashboards and was able to see that it was already receiving data:

![Pasted image 20240513133454](https://github.com/lm3nitro/Projects/assets/55665256/12989a5b-d617-44b6-8d84-a35eda97afa6)

I also took a look at the grid information. This provided me with information about the system itself, the CPU, version that is installed, etc.:

![Pasted image 20240513133551](https://github.com/lm3nitro/Projects/assets/55665256/717496ec-f995-441c-a2c2-f180b8735a04)

As stated in the introduction, Security Onion combines several tools, one of them being the Elastic Stack. The Elastic Stack plays a critical role in the storage, processing, and the visualization of security data. Now that I had everything installed, I can login into Elastic:

![Pasted image 20240513134131](https://github.com/lm3nitro/Projects/assets/55665256/bda54d76-65dd-4f30-87e6-bcf10c916797)

I can see that it is also receiving data:

![Pasted image 20240513134205](https://github.com/lm3nitro/Projects/assets/55665256/9ae918ff-6b60-4172-b30c-33d726e0941d)

InfluxDB is part of Security Onion as well. It is used to store and manage data collected. The data stored in InfluxDB can be visualized and queried through Security Onion's dashboards. I logged into InfluxDB:

![Pasted image 20240513133846](https://github.com/lm3nitro/Projects/assets/55665256/9d92578f-5d99-4659-a2b5-eeda1af49e59)

After logging in, I was able to take a deeper look into the different components:

![Pasted image 20240513133958](https://github.com/lm3nitro/Projects/assets/55665256/f2326f7d-e543-466d-9231-5c4eb8355980)


## Analysis

After exploring the different web interfaces, I wanted to take a deeper look into the traffic and see the capability of Security Onion. To start, I performed some reconnaissance to trigger some alerts using Nmap:

![Pasted image 20240513145414](https://github.com/lm3nitro/Projects/assets/55665256/1ce13b7e-ef96-4f55-af7b-8e4ab958853c)

Output from the scan:

![Pasted image 20240513145501](https://github.com/lm3nitro/Projects/assets/55665256/1d3499fd-55b2-4b3a-a517-5877b7606691)

These are the services and ports opened. Here I see FTP and SSH:

![Pasted image 20240513145528](https://github.com/lm3nitro/Projects/assets/55665256/b6e520d1-d1a9-4939-b0b9-1cf9f26888ce)

Scrolling down, I was also able to see RDP, RPC and SMB also running:

![Pasted image 20240513145605](https://github.com/lm3nitro/Projects/assets/55665256/93aaf04b-d95f-4ccb-bbb3-87a4fd6a8f26)

After the scan had completed, I went back to Security Onion and took a look at some alerts that had been generated on the console. When looking at the alerts, I could see information about the Nmap scan, the services, count, and the severity of the alert:

![Pasted image 20240513142749](https://github.com/lm3nitro/Projects/assets/55665256/c1cd6d2a-2fb8-4e74-96d1-fb3408188b5b)

Security Onion also offers the option to Hunt for within the security alerts. I went with the first alert that references Nmap and is also categorized as critical. Simply right-click and select *Hunt*

![Pasted image 20240513144834](https://github.com/lm3nitro/Projects/assets/55665256/13565a54-5492-4d46-b222-15db54ecee6c)

The **Hunt** option provides more insight into the alert. Here I can see information on the IPs involved in the alert (attacker and target system). It also provides the count, in this case 165, and all the ports that it scanned:

![Pasted image 20240513145035](https://github.com/lm3nitro/Projects/assets/55665256/346a444e-242d-4a34-a00a-7fed4d1533af)

I also queried the alerts in Elastic. Elastic provides a different view into the traffic generated. These are some of the fields that it provides:
+ Type
+ Severity
+ Offset
+ UID and more

![Pasted image 20240513144714](https://github.com/lm3nitro/Projects/assets/55665256/871b0db5-8c4e-4c11-8b26-9dcd1367991d)

### Summary:

In conclusion, this project I installed and configured Security Onion. I then simulated reconnaissance traffic with Nmap against a Metasploitable VM. This was used to test Security Onions detection and logging capabilities. This was primarily aimed to assess Security Onion's effectiveness in identifying security incidents, the ease of use, and the seamless integration with the various tools. I was able to see that Security Onion offers a robust and effective platform for network security monitoring.  It is also effective in detecting and logging various range of security incidents, helping to quickly identify and respond to potential threats and attacks. I was able to gain real hands-on experience in installing and configuring a network security platform. I was also able to manage, monitor, and respond to the security alerts generated within the platform.  


