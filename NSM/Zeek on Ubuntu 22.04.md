# Installing Zeek on Ubuntu 22.04 

I decided to install the latest Zeek 6.2.0 on Ubuntu 22.04. These are the steps that I used to get it installed and configured. In my case, the sniffing inferface on my Ubuntu Server is enp2s0f0. 

I wanted to ensure that Zeek is able to see the full packet data and minimize packet loss. To do this, I applied network sniffing optimizations: settings max ring parameters, disabling NIC offloading, and enabling promiscuous mode.

#### Set max ring parameters on sniffing interface
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
#### Disable NIC offloading
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
#### Set my sniffing interface to promiscuous mode
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

>##### Checked current hardware settings RX:
```
sudo ethtool -g enp2s0f0
```
Output:

<img width="382" alt="Screenshot 2024-04-21 at 9 58 48 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a1dcc688-36a7-45ed-a972-728550e00115">

>##### Checked NIC offloading features were turned off:
```
sudo ethtool -k enp2s0f0
```
Output:

<img width="303" alt="Screenshot 2024-04-21 at 10 02 44 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/48e6eeb5-5209-4f53-aed4-a52fd1d28a02">

>##### Checked PROMISC is listed for my sniffing interface interface:
```
ip a show enp2s0f0 | grep -i promisc
```
Output:

<img width="735" alt="Screenshot 2024-04-21 at 10 03 26 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/473f439d-eaad-4ad3-ac6f-7b1b79daff34">

Next I installed the dependencies needed to install Zeek.

#### Install Zeek Dependencies
```
sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python3 python3-dev python3-git python3-semantic-version swig zlib1g-dev libjemalloc-dev
```
I then ran the following commands to ensure that everything was updated and reboot my server after:
```
sudo apt-get update
sudo apt-get dist-upgrade
sudo reboot
```
Now it's time to install and compile Zeek:

#### Download, Compile, and Install Zeek

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
#### Add Zeek to PATH

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

#### Configure Zeek

To do this, I edited the /opt/zeek/etc/node.cfg to configure the number of workers. For my server, I set up 4 workers. If configuring workers and or if you are using multuple sniffing interfaces, you will want to ensure that the top portion is commented out as seen in the example below:

<img width="563" alt="Screenshot 2024-04-21 at 10 20 15 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/0892049b-e537-4999-af2d-d0727b1b4805">

Now that I have my workers set up, I then started up Zeek.

#### Start Zeek

I ran zeekctl deploy to apply configurations and run Zeek

```
zeekctl deploy
```
Verified that Zeek is running successfully
```
zeekctl status
```
<img width="525" alt="Screenshot 2024-04-21 at 10 25 42 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/55522853-5488-4667-8914-3954a242eea0">

#### ZeekControl Cron

ZeekControl features a cron command to check for and restart crashed workers and to perform other maintenance tasks. To do this, run the following:
```
crontab -e
```
I selected option 1 to use nano and added the following to set up a cron job that runs every five minutes:
```
*/5 * * * * /opt/zeek/bin/zeekctl cron
```
#### Create systemd Zeek Service to Start on Boot

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
#### Optional: Zeek Package Manager

To extend Zeek's functionality, you can take advatage of Zeek's Package Manager. AF_PACKET package is often used to to further optimize packet capture and analysis. Additional useful packages including ja3 and HASSH as well.
To take advantage of Zeek's Packet Manager, will will need to download it To do this, ensure that that it is being done in the home directory where we installed zeek. For me it is in the /opt/:

Used the following to configure Zeek Package Manager:
```
zkg autoconfig
```
This will create a configuration file in /opt/zeek/etc/zkg/config. You can navigate there and ensure that it was created. We can now configure AF_PACKET.

#### Configure Zeek to use AF_PACKET

I then went back to edit my original workers I had set up previously in  /opt/zeek/etc/node.cfg to configure Zeek to use AF_PACKET.  In the example configuration below we are configuring one worker, load balanced across two cores, analyzing one sniffing interface.








