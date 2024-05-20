


The System for internet-Level Knowledge1 (SiLK) tool suite is a highly scalable flow-data capture and analysis system developed by the CERT Situational Awareness group at Carnegie Mellon University’s Software Engineering Institute (SEI). The SiLK tools provide network security analysts with the means to understand, query, and summarize both recent and historical traffic data represented as network flow records (alsoreferred to as “network flow” or “network flow data” and occasionally just “flow”). 

These tools provide network security analysts with a relatively complete high-level view of traffic across an enterprise network, subject to placement of sensors.

Network Topology:


![Pasted image 20240514160825](https://github.com/lm3nitro/Projects/assets/55665256/d28ef60a-2ef7-41aa-bcfa-c7376331de4b)



In this diagram we can see that Silk is placed in the out-of-band network collecting NetFlow records from firewalls and routers on the network, from there the analyst can SSH into the Silk server and queries the data store.

Silk can be also be used as PCAP analysis tool, it could be install on analyst desktop or running as a  NetFlow collector. 




##  Installing SILK:


Updating packages:



![Pasted image 20240509154613](https://github.com/lm3nitro/Projects/assets/55665256/9acfbceb-90f3-4a1b-b42e-b969a0bb3c92)


![Pasted image 20240509154647](https://github.com/lm3nitro/Projects/assets/55665256/10b29cc6-09e0-4d07-be35-e50ececb2e30)



# Install Prerequisites:

install the basic development tools for C compiler required to build SiLK and YAF:

apt install build-essential

![Pasted image 20240509155257](https://github.com/lm3nitro/Projects/assets/55665256/360a7039-383f-49a0-a193-7e5401615ba9)


Run the following command to install the GLib-2, LZO, zlib, GnuTLS, PCAP, and Python development libraries. (For Python and GnuTLS, choose the version numbers that match those currently on your system.) 

apt install libglib2.0-dev liblzo2-dev zlib1g-dev libgnutls28-dev libpcap-dev

![Pasted image 20240509155511](https://github.com/lm3nitro/Projects/assets/55665256/d3ebeb64-e190-4660-a63c-c51ce46107b1)


sudo apt-get install python3-dev

![Pasted image 20240509155635](https://github.com/lm3nitro/Projects/assets/55665256/d83daf54-78ed-410b-8244-24ed62daf709)



To use the MaxMind country-code mapping capability of SiLK, you must install the libmaxminddb-dev package

apt install libmaxminddb-dev

![Pasted image 20240509155741](https://github.com/lm3nitro/Projects/assets/55665256/f57d8ec4-0d45-48b0-8c92-a5c3776131f8)


Download Software:

cd /tmp

wget https://tools.netsa.cert.org/releases/silk-3.19.1.tar.gz

![Pasted image 20240509155914](https://github.com/lm3nitro/Projects/assets/55665256/eab177d2-692d-426d-a463-11c9da408017)


wget https://tools.netsa.cert.org/releases/libfixbuf-2.4.1.tar.gz


![Pasted image 20240509155945](https://github.com/lm3nitro/Projects/assets/55665256/6c18e01e-742e-480a-aa5d-86aa02a2f5cf)


wget https://tools.netsa.cert.org/releases/yaf-2.12.2.tar.gz

![Pasted image 20240509160018](https://github.com/lm3nitro/Projects/assets/55665256/286e843d-5b23-4726-9d66-b3a0187ee924)



Files downloded:

![Pasted image 20240509160122](https://github.com/lm3nitro/Projects/assets/55665256/bead7520-17b0-491d-92a2-f1cee78fc8f2)


Install libfixbuf

Unpack, configure, and install libfixbuf into the /usr/local directory. (This section assumes you downloaded the source code to /tmp.) 

tar -xvzf libfixbuf-2.4.1.tar.gz

![Pasted image 20240509160251](https://github.com/lm3nitro/Projects/assets/55665256/e1eaf337-50e4-4330-83fe-4485b362f701)



 ./configure --prefix=/usr/local --enable-silent-rules



![Pasted image 20240509160455](https://github.com/lm3nitro/Projects/assets/55665256/ef55cde2-4b29-4367-b42e-e55c012c1ff1)



make

![Pasted image 20240509160550](https://github.com/lm3nitro/Projects/assets/55665256/65a795bd-e22a-4f87-81b6-f378afb6b3e9)


make install


![Pasted image 20240509160638](https://github.com/lm3nitro/Projects/assets/55665256/cbae0445-c4b5-47ec-9bfe-a0df20b2a67b)


Install SilK


Note:

 Unpack and configure the SiLK source code. The switches to the configure command do the following:
set the default location for SiLK's hourly repository of flow files to /var/silk/data (The default location is /data.)

enable support for IPv6 addresses and flow records (The default is to support only IPv4 flow records.)

enable creating IPset files in the most compact format (The format may be changed at run-time by providing the --record-version switch to an IPset tool; see also the SILK_IPSET_RECORD_VERSION environment variable.)

enable automatic (de-)compression of binary SiLK files when reading and writing (The default is no compression. The compression may be changed at run-time by providing the --compression-method switch when invoking a tool; see also the SILK_COMPRESSION_METHOD environment variable.)

enable the SiLK Python extension (PySiLK)

enable installation of the PySiLK files under the SiLK installation tree (/usr/local) instead of with the other Python packages

cd /tmp
tar -zxf /tmp/silk-3.19.1.tar.gz
cd silk-3.19.1

./configure --prefix=/usr/local --enable-silent-rules --enable-data-rootdir=/var/silk/data --enable-ipv6 --enable-ipset-compatibility=3.14.0 --enable-output-compression --with-python --with-python-prefix


![Pasted image 20240509161229](https://github.com/lm3nitro/Projects/assets/55665256/7302079e-6dd2-4c55-b27b-f1d8bcd35135)


![Pasted image 20240509161336](https://github.com/lm3nitro/Projects/assets/55665256/c989c7ee-6e40-4ffb-9eb8-cb120fe36dca)



 Now that the code is configured, build and install SiLK. 


make


![Pasted image 20240509161557](https://github.com/lm3nitro/Projects/assets/55665256/4041e747-666b-443a-ba09-c76283b379f2)


make install

![Pasted image 20240509161711](https://github.com/lm3nitro/Projects/assets/55665256/9d80f3c2-2ca5-4ab5-893e-dacf31b32b8d)


Install YAF:

Unpack, configure, and install YAF into the /usr/local directory, enabling YAF's application labeling and deep packet inspection (DPI) capabilities. If you are building yaf-3.0.0 or later, add --enable-dpi to the configure options. 


cd /tmp
tar -zxf /tmp/yaf-2.12.2.tar.gz
cd yaf-2.12.2


./configure --prefix=/usr/local --enable-silent-rules --enable-applabel --enable-metadata --enable-plugins

![Pasted image 20240509162034](https://github.com/lm3nitro/Projects/assets/55665256/59102550-051d-4647-9bf2-c215b8b85a48)


![Pasted image 20240509162110](https://github.com/lm3nitro/Projects/assets/55665256/fb2f61be-8754-4b61-ba36-c1913e04cbeb)


make

![Pasted image 20240509162131](https://github.com/lm3nitro/Projects/assets/55665256/031e8d0b-fb8d-4780-aeba-560415fa4e36)



make install

![Pasted image 20240509162309](https://github.com/lm3nitro/Projects/assets/55665256/2ba4607f-3acd-47fe-9ca9-198741917833)


 Manually copy the YAF start-up script into place. .

cp /tmp/yaf-2.12.2/etc/init.d/yaf /etc/init.d/yaf
chmod a+x /etc/init.d/yaf

![Pasted image 20240509162405](https://github.com/lm3nitro/Projects/assets/55665256/49594c4f-1153-47f6-b938-2ef0cb269580)



Update Dynamic Linker:

 Finally, you should update the cache of the dynamic linker. If you do not, it may be necessary to set the LD_LIBRARY_PATH environment variable to /usr/local/lib when you use SiLK or YAF.

Typically an entry for /usr/local/lib will already exist in the /etc/ld.so.conf.d/ directory. To confirm: 

 
 
 grep local /etc/ld.so.conf.d/*


20240509162541

To update the cache in this case, run the ldconfig program. 


However, if your machine does not have such an entry (or you installed to a location other than /usr/local), you should create. Create a file named silk.conf containing the following line that specifies the library directory for SiLK and YAF: 

/usr/local/lib

 Now copy the file into the /etc/ld.so.conf.d directory and run ldconfig. 


mv silk.conf /etc/ld.so.conf.d/.
ldconfig




# Locating all Silk suite tools:

All installed tools are in /usr/local/bin

![[Attachments/Pasted image 20240509183609.png]]





#  PCAP analysis with Silk :





Taking  packet capture:

![[Attachments/Pasted image 20240510133659.png]]

rwp2yaf2silk - Convert PCAP data to SiLK Flow Records with YAF


![[Attachments/Pasted image 20240510160856.png]]


rwp2yaf2silk is a script to convert a pcap file, such as that produced by tcpdump , to a single file of SiLK Flow records.


The --in and --out switches are required. Note that the --in switch is processed by yaf, and the --out switch is processed by rwipfix2silk.

For information on reading live pcap data and using rwflowpack(8) to store that data in hourly files, see the SiLK Installation Handbook.

Normally yaf groups multiple packets into flow records. You can almost force yaf to create a flow record for every packet so that its output is similar to that of rwptoflow(1): When you give yaf the --idle-timeout=0 switch, yaf creates a flow record for every complete packet and for each packet that it is able to completely reassemble from packet fragments. Any fragmented packets that yaf cannot reassemble are dropped.



First step is to convert pcap file intro network flows. We will use the rwp2yaf2silk command:


rwp2yaf2silk --in=lm3nitro_flow_to_silk.cap --out=lm3nitro_flow_to_silk.silk

![[Attachments/Pasted image 20240510133957.png]]


rwfilter lm3nitro_flow_to_silk.silk --proto=0-255 --pass=stdout --max-pass=2 | rwcut -f 1-10



![[Attachments/Pasted image 20240510134411.png]]


One of the most useful criteria to always look for are the top talkers in the network. Let's search based on the number of flow records with rwstats command:


rwstats --fields=sip --count=20 lm3nitro_flow_to_silk.silk

![[Attachments/Pasted image 20240510134607.png]]

Filtering all the connections for a single IP:

rwfilter --dcidr=151.101.1.140 --pass=stdout lm3nitro_flow_to_silk.silk | rwcut --fields=sip,sport,dip,dport


![[Attachments/Pasted image 20240510135651.png]]

Too many records were returned. Let's count how many are there:

Adding | wc -l 


![[Attachments/Pasted image 20240510135820.png]]


Total of  unique communication with 151.101.1.140 including ports, byte and packets transfer:


rwfilter --dcidr=151.101.1.140 --pass=stdout lm3nitro_flow_to_silk.silk | rwuniq --fields=dport --values=records,bytes,packets --sort-output

![[Attachments/Pasted image 20240510140129.png]]


Extract all TCP flows and display top 5 destination ports by number of flows:

 rwfilter lm3nitro_flow_to_silk.silk  --proto=6 --pass=stdout | rwstats --fields dport --count=5 --flows


![[Attachments/Pasted image 20240510141124.png]]




# Silk information:



Silk Flow Fields:

![[Attachments/Pasted image 20240510155349.png]]

# rwfilter format:



![[Attachments/Pasted image 20240510155652.png]]



# Partition Parameters:

Note: 

At least one partitioning parameter required for every rwfilter:



![[Attachments/Pasted image 20240510155814.png]]




# Ouput:



![[Attachments/Pasted image 20240510160034.png]]



# Silk commands to process output, rwfilter:



![[Attachments/Pasted image 20240510161140.png]]






Linux Firewall:



![[Attachments/Pasted image 20240510212020.png]]


Uncomplicated Firewall (UFW) is a program for managing a netfilter firewall designed to be easy to use.


Search for UFW:


![[Attachments/Pasted image 20240510205540.png]]


Installing UFW:

![[Attachments/Pasted image 20240510205658.png]]

To view status of ufw, type:

ufw status

![[Attachments/Pasted image 20240510205739.png]]

Note:

The default policy firewall works out great for both the servers and desktop. It is always a good policy to closes all ports on the server and open only required ports one by one.


Block all incoming connection and only allow outgoing connections:

ufw default allow outgoing

ufw default deny incoming

![[Attachments/Pasted image 20240510205947.png]]

Open SSH TCP port 22 connections:

![[Attachments/Pasted image 20240510210023.png]]

Open any random TCP port example:

ufw allow 12345/tcp

![[Attachments/Pasted image 20240510210331.png]]

Only allow ssh access from the static IP address such as 10.10.100.1 to our server 10.10.100.105:


ufw allow proto tcp from 10.10.100.1 to 10.10.100.105 port 22

![[Attachments/Pasted image 20240510210411.png]]



Turn on firewall:

ufw enable

![[Attachments/Pasted image 20240510210523.png]]

Cnce UFW enabled, it runs across system reboots too. We can verify that easily as follows using the systemctl command:

![[Attachments/Pasted image 20240510210600.png]]

Disable the UFW based firewall:

![[Attachments/Pasted image 20240510210635.png]]

Open specific incoming connections/ports with description:


ufw allow 80/tcp comment 'lm3nitro Ngnix'

![[Attachments/Pasted image 20240510210727.png]]
ufw allow 443/tcp comment 'HTTPS lm3nitro traffic'

![[Attachments/Pasted image 20240510210816.png]]

Listing all the firewall rules:

ufw status


![[Attachments/Pasted image 20240510210928.png]]


Allow port ranges:

We can allow port ranges too say, tcp and udp 100 to 500:

ufw allow 100:500/tcp
ufw allow 100:500/udp

![[Attachments/Pasted image 20240510211128.png]]


Allow all connections from 10.10.100.10

ufw allow from 10.10.100.10


![[Attachments/Pasted image 20240510211225.png]]

UFW delete rules:


ufw status numbered
![[Attachments/Pasted image 20240510211337.png]]

To delete 7th rule type the command:

ufw delete 7

ufw status numbered

![[Attachments/Pasted image 20240510211515.png]]

Reset the ufw:


ufw reset


Reload the ufw:

ufw reload

![[Attachments/Pasted image 20240510211640.png]]

View the firewall logs:



more /var/log/ufw.log
![[Attachments/Pasted image 20240510211804.png]]

tail -f /var/log/ufw.log

![[Attachments/Pasted image 20240510211827.png]]


Show the list of rules:

ufw show listening


![[Attachments/Pasted image 20240510211855.png]]



ufw show added


![[Attachments/Pasted image 20240510211927.png]]



