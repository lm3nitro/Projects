

# NTOPng











ntopng is a network traffic probe that provides 360° Network visibility, with its ability to gather traffic information from traffic mirrors, NetFlow exporters, SNMP devices, Firewall logs, Intrusion Detection systems.




![[Attachments/Pasted image 20240519152201.png]]


![[Attachments/Pasted image 20240518172238.png]]






![[Attachments/Pasted image 20240518172309.png]]


Enabling universe repository :

sudo add-apt-repository universe

![[Attachments/Pasted image 20240518173605.png]]


Download the latest stable version of ntopng on Ubuntu 20.04.


wget https://packages.ntop.org/apt-stable/20.04/all/apt-ntop-stable.deb

![[Attachments/Pasted image 20240518173505.png]]





Installing the package:

Note: It automatically add the ntopng repository.



![[Attachments/Pasted image 20240518173716.png]]



Update package again:


![[Attachments/Pasted image 20240518173836.png]]




Install ntopng:



sudo apt install pfring-dkms ndpi nprobe ntopng n2disk cento


![[Attachments/Pasted image 20240518173909.png]]


Verify  Ntopng version:



ntopng --version

![[Attachments/Pasted image 20240518174042.png]]


Check status:


sudo systemctl status nprobe cento ntopng pf_ring




![[Attachments/Pasted image 20240518174159.png]]


![[Attachments/Pasted image 20240518174336.png]]



Note: 

If a service isn’t running, start it with systemctl. For example, the centos service is inactive (dead), I need to restart it with:

sudo systemctl restart cento
sudo systemctl status cento

![[Attachments/Pasted image 20240518174435.png]]

Verify the port is listening :


Note: By default, ntopng listens on port 3000.


sudo ss -lnpt | grep 3000

![[Attachments/Pasted image 20240518174553.png]]


Verify the IP address:

The web user interface is available at the http://10.10.100.113:3000


![[Attachments/Pasted image 20240518192114.png]]




Adding and Configuring sniffing interface ens33:


Editing the file located uder /etc/netplan/

![[Attachments/Pasted image 20240518191820.png]]



Disable dhcp client:


![[Attachments/Pasted image 20240518191720.png]]



Apply the changes and verify the interface:

![[Attachments/Pasted image 20240518192013.png]]


![[Attachments/Pasted image 20240518192205.png]]


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


![[Attachments/Pasted image 20240518193143.png]]


Login WebUI:



![[Attachments/Pasted image 20240518174838.png]]





Selecting the interface ens33  on left side conner :


![[Attachments/Pasted image 20240518193355.png]]



After 30 minutes we can see more traffic:


![[Attachments/Pasted image 20240518193336.png]]






Analysing Live traffic:

![[Attachments/Pasted image 20240518193657.png]]




Ntopng provide a blacklisted feature IPs:

In this example we can see the IP that has been blacklisted and it give us the link to verofy the IP to virustotal and AbuseIP DB:



![[Attachments/Pasted image 20240518194957.png]]


Virustotal:


![[Attachments/Pasted image 20240518193915.png]]





Detecting active network scans:


Beacuse the ntopng server is on the DMZ (internet facing) I can see all the scans ongoing on live:



![[Attachments/Pasted image 20240518194736.png]]

Detecting abnormal  live outbound traffic on strange port number :


![[Attachments/Pasted image 20240518194633.png]]





Detecting top talkers destination server:

![[Attachments/Pasted image 20240518202204.png]]




Destination by country:

![[Attachments/Pasted image 20240518205434.png]]



Autonomous Systems information:



![[Attachments/Pasted image 20240518202449.png]]





GeoMAP:


![[Attachments/Pasted image 20240518211214.png]]


Vulnerability Scan:



![[Attachments/Pasted image 20240518203909.png]]




Alerts vuln has been found:

![[Attachments/Pasted image 20240518204149.png]]



Downloading the report:



![[Attachments/Pasted image 20240518205219.png]]



Report:

![[Attachments/Pasted image 20240518204543.png]]


Researching CVE-1999-0661 from a report:



![[Attachments/Pasted image 20240518204648.png]]



Looking at interface ens33 telemetric

![[Attachments/Pasted image 20240518205141.png]]




Filtering traffic per IP:



![[Attachments/Pasted image 20240518210644.png]]

More information about the filtered ip:

![[Attachments/Pasted image 20240518210743.png]]



Taking packet capture from a live traffic:



![[Attachments/Pasted image 20240518205941.png]]


Looking at the ongoing packet capture:

![[Attachments/Pasted image 20240518210859.png]]


Opening one of the packet capture:


![[Attachments/Pasted image 20240518210337.png]]




Summary:



information about Ntopng project:



