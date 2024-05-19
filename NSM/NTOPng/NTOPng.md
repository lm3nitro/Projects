

# NTOPng

![Pasted image 20240518165048](https://github.com/lm3nitro/Projects/assets/55665256/468e6695-e262-453d-a570-c5b13634c626)



ntopng is a network traffic probe that provides 360° Network visibility, with its ability to gather traffic information from traffic mirrors, NetFlow exporters, SNMP devices, Firewall logs, Intrusion Detection systems.

![Pasted image 20240519152201](https://github.com/lm3nitro/Projects/assets/55665256/995cae09-65e0-46bd-98f0-a6a8aca87166)



![Pasted image 20240518172238](https://github.com/lm3nitro/Projects/assets/55665256/3aacf6fc-fa8e-4f2d-8b46-627b160d826a)


![Pasted image 20240518172309](https://github.com/lm3nitro/Projects/assets/55665256/7736267e-5631-481c-9e87-9463b1fcfd43)



Enabling universe repository :

sudo add-apt-repository universe

![Pasted image 20240518173605](https://github.com/lm3nitro/Projects/assets/55665256/a3524f70-df10-43bf-b7c3-0b0936416086)



Download the latest stable version of ntopng on Ubuntu 20.04.


wget https://packages.ntop.org/apt-stable/20.04/all/apt-ntop-stable.deb

![Pasted image 20240518173505](https://github.com/lm3nitro/Projects/assets/55665256/e1bbd80e-a3d8-4b03-8df5-70a57b359bf7)






Installing the package:

Note: It automatically add the ntopng repository.



![Pasted image 20240518173716](https://github.com/lm3nitro/Projects/assets/55665256/be15ac57-d1dd-4e62-bcbf-a6c0fc0a078e)




Update package again:


![Pasted image 20240518173836](https://github.com/lm3nitro/Projects/assets/55665256/61694731-9dd1-4141-8e29-3d328bceb8ce)





Install ntopng:



sudo apt install pfring-dkms ndpi nprobe ntopng n2disk cento


![Pasted image 20240518173909](https://github.com/lm3nitro/Projects/assets/55665256/586e4d29-a9ff-4da2-9853-c68bcd2ad79c)



Verify  Ntopng version:



ntopng --version

![Pasted image 20240518174042](https://github.com/lm3nitro/Projects/assets/55665256/a62d8a3d-5af3-4d3b-9a15-ed3717369649)



Check status:


sudo systemctl status nprobe cento ntopng pf_ring




![Pasted image 20240518174159](https://github.com/lm3nitro/Projects/assets/55665256/802dc199-a371-4913-b5ec-a35cfc76d5e4)


![Pasted image 20240518174336](https://github.com/lm3nitro/Projects/assets/55665256/4d64e168-f6cc-4a40-80b1-8de15e80baf1)




Note: 

If a service isn’t running, start it with systemctl. For example, the centos service is inactive (dead), I need to restart it with:

sudo systemctl restart cento
sudo systemctl status cento

![Pasted image 20240518174435](https://github.com/lm3nitro/Projects/assets/55665256/5e27bb23-894c-433e-a4ad-22a9947eacd3)


Verify the port is listening :


Note: By default, ntopng listens on port 3000.


sudo ss -lnpt | grep 3000

![Pasted image 20240518174553](https://github.com/lm3nitro/Projects/assets/55665256/65f244c3-d494-4432-8f9d-9872a44d564c)



Verify the IP address:

The web user interface is available at the http://10.10.100.113:3000


![Pasted image 20240518192114](https://github.com/lm3nitro/Projects/assets/55665256/bfb7c7c6-0d35-4435-b72c-1e482b86fa23)





Adding and Configuring sniffing interface ens33:


Editing the file located uder /etc/netplan/

![Pasted image 20240518191820](https://github.com/lm3nitro/Projects/assets/55665256/46854a9d-dc80-4e25-b133-c2aaff8939df)




Disable dhcp client:


![Pasted image 20240518191720](https://github.com/lm3nitro/Projects/assets/55665256/a0b0be91-6b59-4baf-b874-9243c991af75)




Apply the changes and verify the interface:

![Pasted image 20240518192013](https://github.com/lm3nitro/Projects/assets/55665256/8a7454df-45ae-4191-912c-ea0f4cb3a85d)


![Pasted image 20240518192205](https://github.com/lm3nitro/Projects/assets/55665256/376416d6-499f-4adf-befa-acd1c4c74ddf)



To configure the interface in promiscuous mode use:


ip link set ens33 off
ip link set ens33 promisc on
ip link set ens33 up

Persistence after reboot configuration:


Adding the following lines to the /etc/rc.local file:

ifconfig ens33 up
ifconfig ens33 promisc



Disabling offloading:

ethtool -K eth0 rx off
ethtool -K eth0 tx off
ethtool -K eth0 sg off
ethtool -K eth0 tso off
ethtool -K eth0 ufo off
ethtool -K eth0 gso off
ethtool -K eth0 gro off
ethtool -K eth0 lro off 


or just on line:


 for i in rx tx sg tso ufo gso gro lro; do ethtool -K ens33 $i off; done


verify the interface :


![Pasted image 20240518193143](https://github.com/lm3nitro/Projects/assets/55665256/49d3ccfb-5852-4012-895f-718b68f9d724)



Login WebUI:



![Pasted image 20240518174838](https://github.com/lm3nitro/Projects/assets/55665256/0e420a71-21d6-41f1-90b7-8f344fcb08ec)






Selecting the interface ens33  on left side conner :


![Pasted image 20240518193355](https://github.com/lm3nitro/Projects/assets/55665256/8f0c373c-10fa-4834-9800-f363d3647e6e)




After 30 minutes we can see more traffic:


![Pasted image 20240518193336](https://github.com/lm3nitro/Projects/assets/55665256/02f0cf9a-dd78-4df6-96a5-c989318e17a9)







Analysing Live traffic:

![Pasted image 20240518193657](https://github.com/lm3nitro/Projects/assets/55665256/c5feea47-9326-4e29-b738-014443340d0c)





Ntopng provide a blacklisted feature IPs:

In this example we can see the IP that has been blacklisted and it give us the link to verofy the IP to virustotal and AbuseIP DB:



![Pasted image 20240518194957](https://github.com/lm3nitro/Projects/assets/55665256/142129ea-c455-40c6-9e53-9414414ee044)



Virustotal:


![Pasted image 20240518193915](https://github.com/lm3nitro/Projects/assets/55665256/d568b1a8-1953-48a0-a19a-8acea28da626)






Detecting active network scans:


Beacuse the ntopng server is on the DMZ (internet facing) I can see all the scans ongoing on live:



![Pasted image 20240518194736](https://github.com/lm3nitro/Projects/assets/55665256/db5938dd-704c-4d2b-a690-b5669d2d9c56)


Detecting abnormal  live outbound traffic on strange port number :


![Pasted image 20240518194633](https://github.com/lm3nitro/Projects/assets/55665256/76753887-c709-4230-b99e-80dd330962cd)






Detecting top talkers destination server:

![Pasted image 20240518202204](https://github.com/lm3nitro/Projects/assets/55665256/4fddcd0c-71ea-471f-ac4a-4ee7ddf56bfc)





Destination by country:

![Pasted image 20240518205434](https://github.com/lm3nitro/Projects/assets/55665256/066ba0a6-20d9-43fc-b778-85b1c604f1df)




Autonomous Systems information:



![Pasted image 20240518202449](https://github.com/lm3nitro/Projects/assets/55665256/1ce50dc3-c1ce-4a1b-aacf-96544e76af3c)






GeoMAP:


![Pasted image 20240518211214](https://github.com/lm3nitro/Projects/assets/55665256/baf0333a-39fb-4219-9114-b5037c6c46e7)



Vulnerability Scan:



![Pasted image 20240518203909](https://github.com/lm3nitro/Projects/assets/55665256/b879bdc7-4641-4172-a2ad-c72230b5242d)





Alerts vuln has been found:

![Pasted image 20240518204149](https://github.com/lm3nitro/Projects/assets/55665256/fe92a4c1-530f-49b0-bb2b-eaaf46f88114)




Downloading the report:



![Pasted image 20240518205219](https://github.com/lm3nitro/Projects/assets/55665256/03058558-5e90-41cb-8a17-1c911b2d53aa)




Report:

![Pasted image 20240518204543](https://github.com/lm3nitro/Projects/assets/55665256/50c8435c-8628-439c-b987-ee4f845b704e)



Researching CVE-1999-0661 from a report:



![Pasted image 20240518204648](https://github.com/lm3nitro/Projects/assets/55665256/6d9ae2c2-1828-47c7-ba03-b1b1a8c64e7f)



Looking at interface ens33 telemetric

![Pasted image 20240518205141](https://github.com/lm3nitro/Projects/assets/55665256/ac07ca9f-3764-4655-8e2c-b2bca87bf27d)





Filtering traffic per IP:



![Pasted image 20240518210644](https://github.com/lm3nitro/Projects/assets/55665256/c4a4df4e-c505-4c15-97cb-1c16d5feb993)


More information about the filtered ip:

![Pasted image 20240518210743](https://github.com/lm3nitro/Projects/assets/55665256/da543f7a-3e2a-4228-96fd-4859ea1802f5)




Taking packet capture from a live traffic:



![Pasted image 20240518205941](https://github.com/lm3nitro/Projects/assets/55665256/d949415b-b21a-4c65-b4b4-3dcdf050eaee)



Looking at the ongoing packet capture:

![Pasted image 20240518210859](https://github.com/lm3nitro/Projects/assets/55665256/d1f839c6-d88e-407a-aa9b-67138b2f3c7a)



Opening one of the packet capture:


![Pasted image 20240518210337](https://github.com/lm3nitro/Projects/assets/55665256/6c8fcbb9-fb8e-44fd-9272-5c3b72168d0f)





Summary:



information about Ntopng project:



