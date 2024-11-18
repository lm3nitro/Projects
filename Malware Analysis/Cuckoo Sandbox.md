# Cuckoo Sandbox

![Pasted image 20240603122550](https://github.com/lm3nitro/Projects/assets/55665256/9942ff97-a422-458f-9f42-52673272c502)

Cuckoo Sandbox is an open-source malware analysis system designed to analyze suspicious files and URLs in a safe, isolated environment (a sandbox). It allows users to observe the behavior of potentially malicious software without risking the host system. By executing the suspicious files in a controlled environment, Cuckoo Sandbox can monitor the file's behavior, interactions with the operating system, and network activity. Below are some of the key features:

+ Automated Malware Analysis
+ Behavioral Analysis
+ Network Traffic Analysis
+ Modular Design
+ Reporting

### Scope:
The primary objective of this lab setup is to provide a controlled environment to analyze, test, and learn about malware. This lab simulates a realistic malware analysis where I will be able to practice identifying, analyzing, and defending against malicious software. The aim is to deepen understanding of malware behavior, system exploitation, and how to design and test defenses. I will walk through the steps that I used to get Cuckoo sandbox installed and configured and will then be testing using a Win7 host. I have provided a topology below to better visualize how this lab will be configured:

![Pasted image 20240603220552](https://github.com/lm3nitro/Projects/assets/55665256/26bc5485-6641-451c-afe9-eb7eaeebc81d)

### Tools and Technology:

Ubuntu, VMWare ESXi, Win7, Cuckoo Sandbox, and Virtual Box

## Getting Started:

To get started, I needed to download the ISO for both the Win7 and Ubuntu:

Windows 7 Ultimate ISO:
![Pasted image 20240602204132](https://github.com/lm3nitro/Projects/assets/55665256/ef26e591-7002-47b2-8718-b6571e8e5339)

> [!TIP]
> Here is the link below for the Win 7 ISO:
> https://drive.google.com/drive/folders/1BsRPYC0v__GiwO8k558FDdGx9IbBAWoP

![Pasted image 20240602204916](https://github.com/lm3nitro/Projects/assets/55665256/cf1a026d-f0a6-4d4d-bb2b-19f0f0a928e2)

ISO Images:

![Pasted image 20240602205202](https://github.com/lm3nitro/Projects/assets/55665256/950c5a48-c6b6-41d1-8600-0fc900de9b95)

## VM Creation:

Now that I have the needed ISOs download, I will start by creating a VM using the newly downloaded Ubuntu ISO on my VMWare ESXi server. I navigated to `Storage > Datastore browser > upload` and selected the Ubuntu 18.04 ISO. 

![Pasted image 20240603115505](https://github.com/lm3nitro/Projects/assets/55665256/eecc0e0e-b335-43f5-b0fb-20a548f90caa)

I then selected to create a new VM, and this is the process I followed and the resources I allocated to the VM:

![Pasted image 20240603120017](https://github.com/lm3nitro/Projects/assets/55665256/3553386a-a8be-4a95-adc5-f4c31e576b01)

![Pasted image 20240603120210](https://github.com/lm3nitro/Projects/assets/55665256/26d09023-252e-4c8d-9683-ddc99b8fdecc)

![Pasted image 20240603120243](https://github.com/lm3nitro/Projects/assets/55665256/e9d34273-d04b-4ec9-8841-4ffc85ab4f95)

![Pasted image 20240603120720](https://github.com/lm3nitro/Projects/assets/55665256/51c0445e-3420-494c-a3cc-ec6fabc6f619)

> [!IMPORTANT]  
>Expose hardware assisted virtualization to the guest OS must be enable. Hardware assisted virtualization is a platform virtualization approach that enables efficient full virtualization using help from hardware capabilities, primarily from the host processors.

![Pasted image 20240603221132](https://github.com/lm3nitro/Projects/assets/55665256/9bda5f83-eeab-4629-8eff-a99d0100cdc8)

## Ubuntu 18.04 Desktop Installation:

Now that the ISO has been uploaded and the VM started, I needed to go through the Ubuntu installation:

![Pasted image 20240603120948](https://github.com/lm3nitro/Projects/assets/55665256/f150858d-1a34-422a-af67-1249b4a3df93)

I selected English:

![Pasted image 20240603121026](https://github.com/lm3nitro/Projects/assets/55665256/09f53a27-0884-4078-9f78-48f12c94a4d1)

Chose Normal Installation:

![Pasted image 20240603121100](https://github.com/lm3nitro/Projects/assets/55665256/11d4c328-3726-45fa-9f76-55f0e8c37a4b)

This is a brand-new install, so I chose 'Erase disk and install Ubuntu'

![Pasted image 20240603121129](https://github.com/lm3nitro/Projects/assets/55665256/3a619fec-6d7d-4ac8-ab7f-b9ea071c190c)

Selected the Eastern Timezone:

![Pasted image 20240603121202](https://github.com/lm3nitro/Projects/assets/55665256/a5b8ea6a-e0f5-47d5-84fc-5cc1d165ce43)

Entered name, username, and password:

![Pasted image 20240603121308](https://github.com/lm3nitro/Projects/assets/55665256/0e6d77d9-d296-44ba-b9d3-f5587d842c4d)

Once installed, I rebooted and was presented with the login screen:

![Pasted image 20240603123115](https://github.com/lm3nitro/Projects/assets/55665256/6ccf7ba3-e4f4-4172-8bb6-ab8fede3222e)

This is just a quick snapshot of the system settings:

![Pasted image 20240603123635](https://github.com/lm3nitro/Projects/assets/55665256/81fef2c7-b33f-45df-b9f7-d60c704766c9)

First things first, I need to update and upgrade:

![Pasted image 20240603122329](https://github.com/lm3nitro/Projects/assets/55665256/fbfb3ecb-8c6d-4d3d-8f4c-2ae9d74eafdb)

![Pasted image 20240603122403](https://github.com/lm3nitro/Projects/assets/55665256/7d33fa3d-63c1-40cb-9168-230542190d1e)

I also needed to install VMWare Tools and OpenSSH:

![Pasted image 20240603122909](https://github.com/lm3nitro/Projects/assets/55665256/63afbe62-f146-4bd0-a23e-daacf0580eb3)

![Pasted image 20240603124709](https://github.com/lm3nitro/Projects/assets/55665256/ad16defb-77cd-4680-a54f-d9b7cdf92bb2)

Staring SSH service and checking the status:

![Pasted image 20240603124840](https://github.com/lm3nitro/Projects/assets/55665256/0f0e5924-5005-4114-aa3d-afc5383d3ea2)

Next, I shut down the VM, and took at snapshot so that I can revert back to a clean slate if needed without having the go through the installation process again. 

![Pasted image 20240603125544](https://github.com/lm3nitro/Projects/assets/55665256/d32df7d6-77e4-4001-8518-b736b29a1aed)

Once I had taken the snapshot, I restarted the VM and SSH'd into it.

![Pasted image 20240603125158](https://github.com/lm3nitro/Projects/assets/55665256/13e1c2d5-2349-4433-a6b4-250a711753ab)

## Installing Dependencies

The following commands were used for the dependencies needed:

```
sudo apt-get install python python-pip python-dev libffi-dev libssl-dev -y
```

![Pasted image 20240603130128](https://github.com/lm3nitro/Projects/assets/55665256/9dde34b4-6982-4136-94cf-e22ae47ef66f)

```
sudo apt-get install python-virtualenv python-setuptools -y
```

![Pasted image 20240603130202](https://github.com/lm3nitro/Projects/assets/55665256/68cd76ca-5155-4625-b095-e3e82548eac0)

```
sudo apt-get install libjpeg-dev zlib1g-dev swig -y
```

![Pasted image 20240603130256](https://github.com/lm3nitro/Projects/assets/55665256/e0787eb0-6ecc-4516-be84-89a4e5264b7e)

```
sudo apt-get install mongodb -y
```

![Pasted image 20240603130325](https://github.com/lm3nitro/Projects/assets/55665256/976205bd-d51b-4919-a2aa-67512bceff5f)

```
sudo apt-get install postgresql libpq-dev -y
```

![Pasted image 20240603130325](https://github.com/lm3nitro/Projects/assets/55665256/dbb3c4a3-0ff5-4b60-a0ec-4daca5bb60c1)

```
sudo apt-get install tcpdump apparmor-utils -y
```

![Pasted image 20240603130457](https://github.com/lm3nitro/Projects/assets/55665256/deb193fd-4744-471f-98c5-8f5255574a64)

I then needed to install VirtualBox. VirtualBox is a free and open-source virtualization software. It allows users to run multiple operating systems simultaneously on a single physical machine by creating virtual machines. Each virtual machine runs a separate instance of an operating system, isolated from the host machine and other VMs. 

![Pasted image 20240603220754](https://github.com/lm3nitro/Projects/assets/55665256/b84fcfc7-c4c8-498e-8bc9-b48dfe1bad3f)

```
sudo apt install virtualbox -y
```

![Pasted image 20240603130417](https://github.com/lm3nitro/Projects/assets/55665256/6ee05785-456e-44a7-9dc9-7f6825a1d985)

Once all the dependencies were installed, I performed another update and upgrade:

```
sudo apt update && sudo apt upgrade -y
```

![Pasted image 20240603130645](https://github.com/lm3nitro/Projects/assets/55665256/eb4e6ff5-9c11-4805-b5c6-a173b33f45a6)

## Adding Groups:

Now that I have the needed dependencies installed, I need to modify the user account permissions. I went ahead and added the user 'cuckoo' to the 'pcap' group:

```
sudo groupadd pcap
sudo usermod -a -G pcap cuckoo
sudo chgrp pcap /usr/sbin/tcpdump
sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
```

![Pasted image 20240603131311](https://github.com/lm3nitro/Projects/assets/55665256/788134c5-c778-4761-8a45-9d7a1cdcbc79)

Verifed permissions:

```
getcap /usr/sbin/tcpdump
```

![Pasted image 20240603131441](https://github.com/lm3nitro/Projects/assets/55665256/6a0e77b6-82dc-46a6-a7f7-7743bb0b4d4b)

Running the following command will disables the AppArmor profile for tcpdump, removing any security restrictions AppArmor has placed on it.

```
sudo aa-disable /usr/sbin/tcpdump
```

![Pasted image 20240603131544](https://github.com/lm3nitro/Projects/assets/55665256/8049f138-16cb-46b4-a596-11c9e44f2aa2)


Next I installed m2crypto:

> [!IMPORTANT]  
> Run all the commands as root user.

```
sudo apt-get install python-dev 
sudo apt-get install python-m2crypto
```

![Pasted image 20240603132519](https://github.com/lm3nitro/Projects/assets/55665256/83ca8cd5-0460-4c3e-b946-b3ca714185f3)

```
pip install m2crypto
```

![Pasted image 20240603132603](https://github.com/lm3nitro/Projects/assets/55665256/9857dea0-3fa0-4f8b-8b36-f4520a8ffdf0)

Once that was installed, I added the user to VirtualBox:

```
sudo usermod -a -G vboxusers cuckoo
```

![Pasted image 20240603132854](https://github.com/lm3nitro/Projects/assets/55665256/c7be090d-9f6d-40aa-b1ac-01a8265f3855)


After, I needed to run the script for virutalenv. Virutalenv is tool for creating isolated virtual python environments.

```
sudo nano cuckoo-setup-virtualenv.sh
```

![Pasted image 20240603133449](https://github.com/lm3nitro/Projects/assets/55665256/4d0d33fa-cc7b-4c1e-9794-64565a80e83c)

Before running the script, this is a view of what it is included in it:

![Pasted image 20240603133211](https://github.com/lm3nitro/Projects/assets/55665256/63b9b62c-3d45-4e61-823f-3b51a26ebcc9)

Changing permission to the script and running the Script:

```
sudo chmod +x cuckoo-setup-virtualenv.sh
```

![Pasted image 20240603142923](https://github.com/lm3nitro/Projects/assets/55665256/35d62cfe-65ec-4c9a-a073-56bdc15ca246)

```
sudo -u cuckoo ./cuckoo-setup-virtualenv.sh
```

![Pasted image 20240603134202](https://github.com/lm3nitro/Projects/assets/55665256/a3505f9c-9fac-4420-92d3-f5bec1746bcd)

Next, I needed to reload bashrc:

```
source ~/.bashrc
```

## Creating a virtualenv:

After running setting up the user permissions, groups, and running the script, I needed to create a virtualenv:

```
mkvirtualenv -p python2.7 cuckoo-test
```

![Pasted image 20240603134612](https://github.com/lm3nitro/Projects/assets/55665256/bf16fefc-87be-42c0-bcfc-72a73fbf697c)

Updating the tools:

```
pip install -U pip setuptools
```

![Pasted image 20240603134933](https://github.com/lm3nitro/Projects/assets/55665256/ce47f75d-9abf-4054-ae59-67c3483004ba)


## Installing Cuckoo:

Time to install Cuckoo:

```
pip install -U cuckoo
```

This is just a quick snap of the initial installation:

![Pasted image 20240603135122](https://github.com/lm3nitro/Projects/assets/55665256/e58320cf-9e67-4a19-95e0-4e0443a407aa)

Another snap of the completed installation:

![Pasted image 20240603135233](https://github.com/lm3nitro/Projects/assets/55665256/384d0bd4-1052-44e2-87e7-f3f6c012ffed)

## Getting into the virtualenv:

![Pasted image 20240603144712](https://github.com/lm3nitro/Projects/assets/55665256/b5d380ac-6fa8-4f05-bef0-ab749d6d39f1)

> [!NOTE]  
> In case that you want to get into the virtualenv use the following command:

```
source /home/cuckoo/.virtualenvs/cuckoo-test/bin/activate
```

Next, I transferred the Win7ultime.iso that I had downloaded in the beginning along with the Ubuntu ISO:

![Pasted image 20240603141918](https://github.com/lm3nitro/Projects/assets/55665256/81baf24f-71d2-433b-ad67-63f533dc73a3)

Once that was uplaoded, I created a directory:

```
sudo mkdir /mnt/win7
```

Added permissions to the directory:

```
sudo chown cuckoo:cuckoo /mnt/win7/
```

Mounted the ISO:

```
sudo mount -o ro,loop win7ultimate.iso /mnt/win7
```

![Pasted image 20240603145003](https://github.com/lm3nitro/Projects/assets/55665256/9b13a47c-8d8a-4f6d-aaed-42fd494385ab)

## Installing VMCloak

After mounting the iso, I Installed VMcloak. VMCloak is a utility for automatically creating Virtual Machines with Windows as guest Operating System.

As always, I needed to install a few dependencies:

```
sudo apt-get -y install build-essential libssl-dev libffi-dev python-dev genisoimage

```

![Pasted image 20240603145344](https://github.com/lm3nitro/Projects/assets/55665256/ec2a77ef-8f44-46bd-a9f9-8690cf2daade)

```
sudo apt-get -y install zlib1g-dev libjpeg-dev
```

![Pasted image 20240603145429](https://github.com/lm3nitro/Projects/assets/55665256/cc0a1b82-8476-4149-b248-9746d2a3dc3e)

```
sudo apt-get -y install python-pip python-virtualenv python-setuptools swig
```

![Pasted image 20240603145518](https://github.com/lm3nitro/Projects/assets/55665256/acbd8618-7c84-49af-9a4b-bf377e96da26)

Once the dependencies were installed, I continued with VMCloak:

```
pip install -U vmcloak
```

![Pasted image 20240603145649](https://github.com/lm3nitro/Projects/assets/55665256/9f6dec06-6243-4263-b45a-0e2c28d156e3)


## Creating a Virtual Network:

The next step was configuring the virtual network for the VM:
vmcloak-vboxnet0

![Pasted image 20240603145835](https://github.com/lm3nitro/Projects/assets/55665256/7004dcf6-8f00-4412-96d6-38b053b2b0e4)


These are the virtual machine specs:

```
vmcloak init --verbose --win7x64 win7x64base --cpus 4 --ramsize 8192
```

![Pasted image 20240603152522](https://github.com/lm3nitro/Projects/assets/55665256/0ad480ee-f540-4cd2-9aae-674d59d8d2ee)

Cloning the VM:

```
vmcloak clone win7x64base win7x64cuckoo
```

![Pasted image 20240603152716](https://github.com/lm3nitro/Projects/assets/55665256/004d4263-fee4-4268-a3f1-a5daa50d3b92)


## Installing all the software for the Windows 7:

```
vmcloak list deps
```

![Pasted image 20240603152844](https://github.com/lm3nitro/Projects/assets/55665256/27137937-1cbe-4a54-9c20-7e61ffb62848)

Installing Internet Explore 11:

```
vmcloak install win7x64cuckoo ie11
```

![Pasted image 20240603153455](https://github.com/lm3nitro/Projects/assets/55665256/30f11a2b-ae59-4070-a450-34fd0dcb91cf)

## Creating Snapshot:

Going forward, I wanted to ensure that I had a snapshot of the Win7 host. To do this, I used the following command:

```
vmcloak snapshot --count 1 win7x64cuckoo 192.168.56.101
```

![Pasted image 20240603154237](https://github.com/lm3nitro/Projects/assets/55665256/665597af-f787-40f1-b691-90309ca86a56)

To verify, I checked all the VMs:

```
vmcloak list vms
```

![Pasted image 20240603154321](https://github.com/lm3nitro/Projects/assets/55665256/233561cc-cae9-4451-8dc1-830d36f9aa07)


## Allow VM traffic:

Now I needed to allow the VM traffic. First, I needed to identify the interfaces:

![Pasted image 20240603154602](https://github.com/lm3nitro/Projects/assets/55665256/967f6660-eca3-4789-ab66-896bdb7b2bbe)

I set up the fowarding:

```
sudo sysctl -w net.ipv4.conf.vboxnet0.forwarding=1
```

![Pasted image 20240603154633](https://github.com/lm3nitro/Projects/assets/55665256/19a0fe35-4333-4bc0-ad16-c5b740938375)

```
sudo sysctl -w net.ipv4.conf.ens192.forwarding=1
```

![Pasted image 20240603154808](https://github.com/lm3nitro/Projects/assets/55665256/d50b53a3-d760-4465-b403-28e8fb62cf3a)

I then added persistence so that these changes are still applied after reboot. To do this, I went to the `sysctl.conf`:

```
sudo nano /etc/sysctl.conf
```

I added the following lines:

```
net.ipv4.conf.vboxnet0.forwarding=1
net.ipv4.conf.ens192.forwarding=1
```

![Pasted image 20240603155009](https://github.com/lm3nitro/Projects/assets/55665256/06ff5f8c-2423-4254-b646-24bef260e268)

## NAT with IPtables:

After applying the changes, I need to set NAT using IPtables:

```
sudo iptables -t nat -A POSTROUTING -o ens192 -s 192.168.56.0/24 -j MASQUERADE
sudo iptables -P FORWARD DROP
sudo iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -s 192.168.56.0/24 -j ACCEPT
```

![Pasted image 20240603155336](https://github.com/lm3nitro/Projects/assets/55665256/40b27f85-941c-4922-aa00-503fc1751424)

Verified IPtables:

```
sudo iptables -vnL
```

![Pasted image 20240603155358](https://github.com/lm3nitro/Projects/assets/55665256/377f4e32-a8dd-4a83-892d-908b9669811d)

> [!TIP]
> Another way to get into the virtualenv use: `workon "name_of _the_virtualenv"`
> ![Pasted image 20240603155957](https://github.com/lm3nitro/Projects/assets/55665256/e7f59d65-6156-4dfa-8d39-327267f360f9)

## Working with Cuckoo:

Tmux allows you to manage multiple terminal sessions within a single window, enabling easy switching between different tasks. I personally like it for multitasking, and running commands in the background, especially when working remotely or on servers.

Installing Tmux:

```
sudo apt install tmux
```

![Pasted image 20240603155822](https://github.com/lm3nitro/Projects/assets/55665256/24ef0c60-174f-4082-b999-09eb70465833)

Next, I went into the virtualenv and initialized a new Cuckoo Sandbox setup using the command below:

```
cuckoo init
```

![Pasted image 20240603160133](https://github.com/lm3nitro/Projects/assets/55665256/9c2af835-78cf-4f0a-b0bd-3c0307785e9b)

> [!NOTE]  
> Cuckoo configuration, Yara rules, Cuckoo Signatures, and much more under can be found in the following directory: `/home/cuckoo/.cuckoo`
> Location for the Cuckoo configuration file: `/home/cuckoo/.cuckoo/conf`

Next, I proceeded to download the community rules:

```
cuckoo community
```

![Pasted image 20240603160602](https://github.com/lm3nitro/Projects/assets/55665256/558aa26a-c005-4355-89bd-a7e86c28a390)

I then needed to configure and runs Cuckoo's rooter utility, which is responsible for managing network-related tasks that require elevated privileges. This helps Cuckoo Sandbox handle network operations securely:

```
cuckoo rooter --sudo --group cuckoo
```

![Pasted image 20240603161142](https://github.com/lm3nitro/Projects/assets/55665256/b42c983f-e6c4-411b-8340-ac1615bfcc9b)

Now I went into the VirtualBox conf:

```
sudo nano virtualbox.conf
```

![Pasted image 20240603161332](https://github.com/lm3nitro/Projects/assets/55665256/d64f6b97-1ac7-4537-8a8b-d01371a83e56)

Using his command automates the process of adding VMs (created with Vmcloak) into Cuckoo Sandbox by reading the VM names and IP addresses from the vmcloak list vms output and registering each one with Cuckoo Sandbox for malware analysis:

```
while read -r vm ip; do cuckoo machine --add $vm $ip; done < <(vmcloak list vms)
```

![Pasted image 20240603161736](https://github.com/lm3nitro/Projects/assets/55665256/cf6a6155-5dd3-465b-bb85-a8118a89f56f)

I then removed the cuckoo1 block:

![Pasted image 20240603161845](https://github.com/lm3nitro/Projects/assets/55665256/a0f828b6-01f3-4776-b571-848371e24317)

It should look like this after the removal:

![Pasted image 20240603162859](https://github.com/lm3nitro/Projects/assets/55665256/87be7674-f8c6-44c4-989b-82b0b98edd74)

Next, I configured the routing. To do this I navigated to routing.conf and added the host interface:

```
nano routing.conf
```

![Pasted image 20240603163059](https://github.com/lm3nitro/Projects/assets/55665256/e6fa0e4e-6b41-4f06-8aa1-6c46889ba2ac)

I the configured reporting:

```
nano reporting.conf
```

Note: Change the mongodb block to "yes"

![Pasted image 20240603163332](https://github.com/lm3nitro/Projects/assets/55665256/7bdaffef-178e-4346-b8d5-2af03539a057)

## Running Cuckoo:

To start Cuckoo, use `cuckoo` or `cuckoo -d`

```
cuckoo
```

![Pasted image 20240603163642](https://github.com/lm3nitro/Projects/assets/55665256/a8ec8918-829c-4e53-b35a-08c76f9f2412)

Starting the Web UI. First, I verified the IP address of the host interface:

![Pasted image 20240603163959](https://github.com/lm3nitro/Projects/assets/55665256/79f183d7-30c6-46c7-96c6-30458813c033)

```
cuckoo web --host 10.10.100.39 --port 8080
```

![Pasted image 20240603164035](https://github.com/lm3nitro/Projects/assets/55665256/5aaad83d-a958-4865-8d0f-611bac7b888a)

```
http://lm3nitro.cuckoo.malware.local:8080/
```

Output from the server:

![Pasted image 20240603164659](https://github.com/lm3nitro/Projects/assets/55665256/e40b6ceb-c61d-4428-a51b-d296699f6ec5)

Accessing the web interface:

![Pasted image 20240603164518](https://github.com/lm3nitro/Projects/assets/55665256/4b82881c-df81-45ca-997a-b3d1beb07679)

## Testing

Since I do not have a malware that I can use for testing, I used the EICAR anti-malware test file:

![Pasted image 20240603165316](https://github.com/lm3nitro/Projects/assets/55665256/a935eafc-8b1c-48db-ac08-8e2a2a8d2d71)

Once I downloaded the file, I uploaded it to the Cuckoo sandbox and analyzed it:

![Pasted image 20240603165525](https://github.com/lm3nitro/Projects/assets/55665256/98d87511-9a14-4a95-952a-325c66ec9c0e)

In the backend, I could see the analysis was being performed:

![Pasted image 20240603165812](https://github.com/lm3nitro/Projects/assets/55665256/8573c73c-460b-4bb4-a02a-4f7089316be2)

Analysis running: 

![Pasted image 20240603183540](https://github.com/lm3nitro/Projects/assets/55665256/e708ef79-136d-43ce-afdf-4e167d14ba43)

Once the analysis completed, these were the results. It showed a 2.2/10 score:

![Pasted image 20240603165908](https://github.com/lm3nitro/Projects/assets/55665256/47f4b131-fee8-43ac-8ccd-6746dc467986)

Taking a look at the summary, I can see that it found several hashes associated with the file. It also found that it has resumed a suspended thread potentially indicative of a process injection:

![Pasted image 20240603183816](https://github.com/lm3nitro/Projects/assets/55665256/9d632948-cf60-458e-ae0f-91cbf8feccfe)

For further verification, I checked the hash provided using VirusTotal:

![Pasted image 20240603184018](https://github.com/lm3nitro/Projects/assets/55665256/be5e1d5a-9408-40fb-96cf-78f1da800e0a)

Below is a more information on the resumed suspended thread:

![Pasted image 20240603184545](https://github.com/lm3nitro/Projects/assets/55665256/eb02a33e-4599-4e05-acab-9e43332ec4a4)

Closer look at the details on the process injection:

![Pasted image 20240603184437](https://github.com/lm3nitro/Projects/assets/55665256/c8efeb7d-eb38-4679-97ec-712c328832bc)

Looked at the analyzer logs. The analyzer log is a key log file that provides detailed information about the dynamic analysis process conducted within the virtual machine (behavior of the target file or URL being analyzed).

![Pasted image 20240603184309](https://github.com/lm3nitro/Projects/assets/55665256/06d42fa5-6a1b-4227-afa5-47fba04d8be4)

Further Analysis:

![Pasted image 20240603184111](https://github.com/lm3nitro/Projects/assets/55665256/2f436698-7c19-4028-bc4e-dd8836bbcc01)

I also wanted to test another file. I sed the following:

![Pasted image 20240603185538](https://github.com/lm3nitro/Projects/assets/55665256/bd212849-15ae-4f1c-8381-e8df94057d68)

Uploaded it and analyzed it:

![Pasted image 20240603185810](https://github.com/lm3nitro/Projects/assets/55665256/fc6faab9-f414-427b-bdcc-68dbb0b02b01)

Just a quick glance at the analysis in the backend:

![Pasted image 20240603190734](https://github.com/lm3nitro/Projects/assets/55665256/e365568b-d17c-4ce6-80a3-e9a1d6f13dfb)

This file had a couple more signatures:

![Pasted image 20240603190031](https://github.com/lm3nitro/Projects/assets/55665256/04f0ee87-8e8c-4e7b-8f95-97d8861c4634)

Taking a closer look at the signatures:

![Pasted image 20240603190108](https://github.com/lm3nitro/Projects/assets/55665256/421c8cc7-916a-47f2-8dbf-49f43a58d548)

Also cross-referenced and verifies using VirusTotal:

![Pasted image 20240603190152](https://github.com/lm3nitro/Projects/assets/55665256/2513d086-c644-4b52-8526-429e89acaf7c)


### Summary:

This project took longer than originally anticipated, but definitely worth it. In this project, that following setup was accomplished:

+ Host Machine Setup: I created an Ubuntu 18.04 VM in my VMWare ESXi Hypervisor Type 1. This was used as the base operating system for the Cuckoo Sandbox controller.
+ Installation of Cuckoo Sandbox, its dependencies, and configuration.
+ Configured network settings to ensure isolation between the virtual environment and the rest of the network.
+ Guest Virtual Machine Setup; Installed Windows 7 in VirtualBox
+ Ensured snapshot functionality was enabled, so that after each malware test, the VM can be reset to a clean state.
+ Downloaded malware samples to test cuckoo sandbox analysis

Going through the complete installation and configuration provided critical hands-on experience. I was able to gain an understanding of how a sandbox environment works to analyze malware behavior in isolation. This includes learning how to run and analyze various types of files in a controlled manner. By completing this project, I was also able to do the following:

+ Gained practical understand of malware analysis
+ Built a virtualized and isolated environment: This is essential in order to experiment and test without affecting live systems
+ Troubleshooting and problem solving: I will be honest; the installation did not go 100% smoothly on the first try. I had to learn how to research and troubleshoot the issues I encountered
+ Security configurations and hardening: I was able to better understand why network isolation is essential to prevent malware from escaping the sandbox.
 
Overall, this helped me to cultivate a defensive mindset and understand the attacker's mindset. The challenges encountered during the installation and configuration also helped with my critical thing and problem-solving skills. This was a great hands-on experience in malware analysis, virtualization, networking, and system security. I would recommend anyone who wants to dive into cybersecurity to have their own sandbox. 
