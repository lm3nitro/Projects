# Installing Zeek on Ubuntu 22.04 

![Screenshot 2024-04-27 at 10 49 02 PM](https://github.com/lm3nitro/Projects/assets/55665256/e0facd98-1e90-4ba0-ac2e-6cdf16decd24)

Zeek is an open-source network monitoring tool used for security analysis and network traffic inspection. Some key features include:
+ Traffic Analysis
+ Security Monitoring
+ Extensibility
+ Logging
+ Connection Tracking and more

Aside from the features, Zeek also has some important use cases such as incident response, network forensics, and threat hunting. 

### Scope:
I will be installing Zeek on Ubuntu 22.04 as part of my home network. I will be covering the installation and configuration of Zeek, and will then configure Zeek to send the its generated logs to my Splunk instance. 

### Tools and Technology:
Zeek, Linux, and Splunk

## Install 

I decided to install the latest Zeek 6.2.0 on Ubuntu 22.04. These are the steps that I used to get it installed and configured. In my case, the sniffing interface on my Ubuntu Server is enp2s0f0. 

I wanted to ensure that Zeek is able to see the full packet data and minimize packet loss. To do this, I applied network sniffing optimizations: settings max ring parameters, disabling NIC offloading, and enabling promiscuous mode.

1. Set max ring parameters on sniffing interface

I created a new file in /etc/networkd-dispatcher/routable.d/10-set-max-ring and add the following lines for my sniffing interface:
```
#!/bin/sh
# Set ring rx parameters for all sniffing interfaces
ethtool -G enp2s0f0 rx 4096
```
Saved the file and set its permissions to 755:
```
chmod 755 /etc/networkd-dispatcher/routable.d/10-set-max-ring
```
2. Disable NIC offloading

Created a new file in /etc/networkd-dispatcher/routable.d/20-disable-checksum-offload and add the following for my sniffing interface:
```
#!/bin/sh
# Disable checksum offloading for all sniffing interfaces
ethtool -K enp2s0f0 rx off tx off sg off tso off ufo off gso off gro off lro off
```
Saved the file and set its permissions to 755:
```
chmod 755 /etc/networkd-dispatcher/routable.d/20-disable-checksum-offload
```
3. Set my sniffing interface to promiscuous mode

```
#!/bin/sh
# Enable promiscuous mode for all sniffing interfaces
ip link set enp2s0f0 arp off multicast off allmulticast off promisc on
```
Saved the file and set its permissions to 755:

```
chmod 755 /etc/networkd-dispatcher/routable.d/30-enable-promisc-mode
```
I then rebooted my server and verified that my configurations remained persistent. To check the sniffing interface parameters, I used the following commands:

4. Checked current hardware settings RX:
```
sudo ethtool -g enp2s0f0
```
Output:

<img width="382" alt="Screenshot 2024-04-21 at 9 58 48 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a1dcc688-36a7-45ed-a972-728550e00115">

5.  Checked NIC offloading features were turned off:
```
sudo ethtool -k enp2s0f0
```
Output:

<img width="303" alt="Screenshot 2024-04-21 at 10 02 44 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/48e6eeb5-5209-4f53-aed4-a52fd1d28a02">

6. Checked PROMISC is listed for my sniffing interface interface:
```
ip a show enp2s0f0 | grep -i promisc
```
Output:

<img width="735" alt="Screenshot 2024-04-21 at 10 03 26 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/473f439d-eaad-4ad3-ac6f-7b1b79daff34">

Next, I installed the dependencies needed to install Zeek.

## Installing Zeek Dependencies

1. Dependencies:
```
sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python3 python3-dev python3-git python3-semantic-version swig zlib1g-dev libjemalloc-dev
```

2. I then ran the following commands to ensure that everything was updated and reboot my server after:
```
sudo apt-get update
sudo apt-get dist-upgrade
sudo reboot
```
Now it's time to install and compile Zeek:

3. Download, Compile, and Install Zeek

Here I download zeek to the /opt/ directory and enabled jemmaloc to improve memory and CPU usage:
```
cd
wget https://download.zeek.org/zeek-6.2.0.tar.gz
tar -xzvf zeek-6.2.0.tar.gz
cd zeek-6.2.0
./configure --prefix=/opt/zeek --enable-jemalloc --build-type=release
make
make install
```
4. Add Zeek to PATH
I then proceeded to add Zeek to Path. In order to do this, I navigated to the ~/.bashrc file and added the following line at the bottom:

```
# Add Zeek to PATH
export PATH="/opt/zeek/bin:$PATH"
```
<img width="650" alt="Screenshot 2024-04-21 at 10 15 00 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/e5c6737c-426b-476b-b2d6-3d637910a63a">
I then saved the file and applied the new path:
```
source ~/.bashrc
```
I then moved to the Zeek configuration.

## Configure Zeek

To do this, I edited the /opt/zeek/etc/node.cfg to configure the number of workers. For my server, I set up 4 workers. If configuring workers and or if you are using multuple sniffing interfaces, you will want to ensure that the top portion is commented out as seen in the example below:

<img width="563" alt="Screenshot 2024-04-21 at 10 20 15 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/0892049b-e537-4999-af2d-d0727b1b4805">

Now that I have my workers set up, I then started up Zeek.

## Start Zeek
I ran zeekctl deploy to apply configurations and run Zeek

```
zeekctl deploy
```
Verified that Zeek is running successfully
```
zeekctl status
```
<img width="525" alt="Screenshot 2024-04-21 at 10 25 42 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/55522853-5488-4667-8914-3954a242eea0">

1. ZeekControl Cron

ZeekControl features a cron command to check for and restart crashed workers and to perform other maintenance tasks. To do this, run the following:
```
crontab -e
```
I selected option 1 to use nano and added the following to set up a cron job that runs every five minutes:
```
*/5 * * * * /opt/zeek/bin/zeekctl cron
```
2. Create systemd Zeek Service to Start on Boot

I created a systemd service that enabled me to start Zeek at boot time and easily start/stop whenever necessary. To do this, I created a new service file in /etc/systemd/system/zeek.service and added the following lines:
```
[Unit]
Description=Zeek
[Service]
User=root
Type=forking
Restart=always
RestartSec=1
StartLimitAction=reboot
ExecStart=/opt/zeek/bin/zeekctl deploy
ExecStop=/opt/zeek/bin/zeekctl stop
[Install]
WantedBy=multi-user.target
```
Saved the file. Now I am able to easily start, stop, or check the status of the Zeek service by running the following commands:
```
sudo systemctl start zeek
sudo systemctl stop zeek
sudo systemctl status zeek
```
Finally, to enable Zeek to run at system boot time, I ran the following command:
```
sudo systemctl enable zeek
```
3. Optional: Zeek Package Manager

To extend Zeek's functionality, you can take advantage of Zeek's Package Manager. AF_PACKET package is often used to to further optimize packet capture and analysis. Additional useful packages including ja3 and HASSH as well.

To take advantage of Zeek's Packet Manager, I will need to download it. To do this, I ensured that that it is being done in the home directory where I installed Zeek. For me it is in the /opt/:

Used the following to configure Zeek Package Manager:
```
zkg autoconfig
```
This will create a configuration file in /opt/zeek/etc/zkg/config. You can navigate there and ensure that it was created. We can now configure AF_PACKET.

4. Configure Zeek to use AF_PACKET

I then went back to edit my original workers I had set up previously in /opt/zeek/etc/node.cfg to configure Zeek to use AF_PACKET.  In the example configuration below I am configuring one worker, load balanced across two cores, analyzing one sniffing interface.

```
# Example ZeekControl node configuration.
# Below is an example clustered configuration on a single host.
[logger]
type=logger
host=localhost
[manager]
type=manager
host=localhost
[proxy-1]
type=proxy
host=localhost
[worker-1]
type=worker
host=localhost
interface=af_packet::enp2s0f0
lb_method=custom
lb_procs=2
pin_cpus=0,1
[worker-2]
type=worker
host=localhost
interface=af_packet::enp2s0f0
lb_method=custom
lb_procs=2
pin_cpus=0,1
[worker-3]
type=worker
host=localhost
interface=af_packet::enp2s0f0
lb_method=custom
lb_procs=2
pin_cpus=0,1
[worker-4]
type=worker
host=localhost
interface=af_packet::enp2s0f0
lb_method=custom
lb_procs=2
pin_cpus=0,1
```
Since the configuration was changed to add AF_PACKET, I ran zeekctl deploy to apply the new configuration changes and run Zeek.
```
zeekctl deploy
```

## Zeek logs to Splunk

As part of this project, I then wanted to configure Zeek to send the logs to my Splunk. For this particular lab, I went ahead and used the Corelight App. To do this, I did the following:

+ Configured Zeek to output logs in JSON format for consumption by Splunk.
+ Created an index in Splunk for Zeek data.
+ Installed and configured the `Corelight For Splunk` app to index and parse Zeek logs in Splunk.
+ Created a splunk user to run the Splunk Universal Forwarder.
+ Installed and configured a Splunk Universal Forwarder to send Zeek logs to a Splunk instance.

1. Output Zeek logs to JSON
Stop Zeek if it is currently running
```
zeekctl stop
```
Edit the /opt/zeek/share/zeek/site/local.zeek and add the following:
```
# Output to JSON
@load policy/tuning/json-logs.zeek
```
<img width="629" alt="Screenshot 2024-04-21 at 10 52 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/bd20b775-89d5-403c-9cba-ec504a01d0a9">

Restarted Zeek and view the logs in /opt/zeek/logs/current to confirm they are now in JSON format:
```
zeekctl deploy
cd /opt/zeek/logs/current
less conn.log
```

2. Create an index in Splunk for Zeek data

To do this, I went to my Splunk instance and navigated to Settings > Data > Indexes, and created a new index for Zeek:

<img width="1121" alt="Screenshot 2024-04-21 at 10 54 32 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a99911d6-fc17-4b68-93cf-dd24a32774b8">

3. Install and configure the `Corelight For Splunk` app
To do this I first went to the Splunk Apps and downloaded the Corelight App for Zeek:

<img width="1090" alt="Screenshot 2024-04-21 at 8 03 24 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/715f0184-352e-4ff8-a50e-23babb0adda3">

Once I had it downloaded, I then uploaded it to Splunk via the file method:
<img width="1429" alt="Screenshot 2024-04-21 at 10 59 41 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/57fa9e8a-7789-499c-b37b-b03ffe624d50">

Once the Application was installed, I needed to ensure that it was configured to the correct index, the Zeek index I previously created. I went to Settings > Knowledge > Event types:

<img width="1420" alt="Screenshot 2024-04-21 at 11 09 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/7e015ab4-3916-4f85-b81a-8acd04f09d08">

4. I then configured the corelight_idx to point to my zeek index:

<img width="1292" alt="Screenshot 2024-04-21 at 11 12 54 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/354636b4-9c34-4484-94c2-c1e5c8292156">

Now that I have that setup in splunk, I then moved on to install the Splunk Fowarder in the Ubuntu Server where I installed Zeek.

## Install and configure a Splunk Universal Forwarder

I navigated to Splunk and went to download the latest Splunk Fowarders, at this time it is 9.2.1. I did this by getting the wget command provided:

<img width="1421" alt="Screenshot 2024-04-21 at 11 17 08 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/14102a80-077c-4d93-90af-99c94d498637">

Once I had the wget command copied, I then navigated back to my /opt/ directory and pasted it:
```
wget -O splunkforwarder-9.2.1-78803f08aabb-linux-2.6-amd64.deb "https://download.splunk.com/products/universalforwarder/releases/9.2.1/linux/splunkforwarder-9.2.1-78803f08aabb-linux-2.6-amd64.deb"
```
Unpackaged it:
```
dpkg -i splunkforwarder-9.2.1-78803f08aabb-linux-2.6-amd64.deb
```
Once that completed, I accepted the license agreement and created an administrative username and password:
```
cd /opt/splunkforwarder/bin
./splunk start --accept-license
```

Once that was completed, I needed to make some configuration changes to the fowarder and needed to stop it:
```
./splunk stop
```
I then went on to configure the inputs.conf file. This file does not exit by default, so I needed to create it:
```
nano /opt/splunkforwarder/etc/system/local/inputs.conf
```
Since I am using the corelight app, I replaced the “zeek” sourcetype prefix with “corelight” as this is what the app is expecting (e.g. replace “zeek_conn” with “corelight_conn”). See the example below:
```
[default]
host = sensor
[monitor:///opt/zeek/logs/current/conn.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_conn
[monitor:///opt/zeek/logs/current/dns.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_dns
[monitor:///opt/zeek/logs/current/software.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_software
[monitor:///opt/zeek/logs/current/smtp.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_smtp
[monitor:///opt/zeek/logs/current/ssl.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_ssl
[monitor:///opt/zeek/logs/current/ssh.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_ssh
[monitor:///opt/zeek/logs/current/x509.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_x509
[monitor:///opt/zeek/logs/current/ftp.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_ftp
[monitor:///opt/zeek/logs/current/http.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_http
[monitor:///opt/zeek/logs/current/rdp.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_rdp
[monitor:///opt/zeek/logs/current/smb_files.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_smb_files
[monitor:///opt/zeek/logs/current/smb_mapping.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_smb_mapping
[monitor:///opt/zeek/logs/current/snmp.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_snmp
[monitor:///opt/zeek/logs/current/sip.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_sip
[monitor:///opt/zeek/logs/current/files.log]
_TCP_ROUTING = *
index = zeek
sourcetype = corelight_files
```
I also needed to create the outputs.conf to send data to my Splunk server. In the sample file below, I replaced each instance of splunkserver:9997 with my own server name/IP and port number.
```
[tcpout]
defaultGroup = default-autolb-group
[tcpout:default-autolb-group]
server = splunkserver:9997
[tcpout-server://splunkserver:9997]
```
I then started the forwarder and confirmed that it was running and that I was receiving the data in Splunk:

```
cd /opt/splunkforwarder/bin
./splunk start
```
<img width="1419" alt="Screenshot 2024-04-21 at 11 40 58 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b1aa3999-a7f4-4a4c-b02f-e299dc6ef2d7">

Verification:

<img width="1429" alt="Screenshot 2024-04-21 at 11 46 25 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d792abee-0155-4060-8635-875b07322157">
<img width="1422" alt="Screenshot 2024-04-21 at 11 50 34 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/48e8efe0-456d-4803-aab5-1aef5c6715e2">

### Summary: 

Installing Zeek and having it running in my network has big benefits. Having this installed provides deep visibility into my network traffic, helping to detect anomalies, potential intrusions, and suspicious behaviors. While working with Zeek, I have gained experience in network security monitoring which has enabled me to practice threat detection, learn how to investigate network events, and build upon my understanding of cybersecurity best practice and concepts. I was able to learn and analyze real-time network communication monitoring along with analysis of patterns and behaviors. Having Zeek has also allowed me to gain hands-on experience in reconstructing past events by examining its logs for root cause analysis. I highly recommend having Zeek installed in your home network. 





