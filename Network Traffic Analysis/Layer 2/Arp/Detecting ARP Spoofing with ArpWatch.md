# Detecting ARP Spoofing with ArpWatch


**Arpwatch** is an open source computer software program that helps you to monitor **Ethernet** traffic activity (like **Changing IP** and **MAC Addresses**) on your network and maintains a **database of ethernet/ip address pairings**. It produces a log of noticed pairing of IP and MAC addresses information along with a timestamps, so you can carefully watch when the pairing activity appeared on the network. It also has the option to send reports via email to an network administrator when a pairing added or changed.

# Installing Arpwatch: 


apt install arpwatch

![Pasted image 20240404191910](https://github.com/lm3nitro/Projects/assets/55665256/a42e6239-19ba-429c-8a94-3bf07ab291ba)


### Arpwatch configuration


Go to **_/etc/arpwatch_** directory and create file `enp5s0.iface` (ens32.iface) with this content:

In order to do that, create a file called ens32.iface` which contains variable assignments in **sh** syntax (comments are allowed). You can use the following variables to influence the invocation for that specific interface only:

- **ARGS**: overwrite the ARGS from _**/etc/default/arpwatch**_
- **PCAP_FILTER**: overwrite (or set) the pcap filter
- **IFACE_ARGS**: additional options to be passed to arpwatch


- The **-m** option is used to specify the e-mail address to which reports will be sent. By default, reports are sent to root on the local machine.
    
- The **-n** flag specifies additional local networks. This can be useful to avoid **bogon** warnings when there is more than one network running on the same wire. If the optional width/mask is not specified, the default netmask for the network's class is used.
    
- The **-N** flag disables reporting any bogons.
    
- The **-p** flag disables **promiscuous operation**. ARP broadcasts get through hubs without having the interface in promiscuous mode, while saving considerable resources that would be wasted on processing gigabytes of non-broadcast traffic.




Example:

```
INTERFACES="ens32"
ARGS="-N -p"
IFACE_ARGS="-m erick.gomez@lm3nitro.com -n 192.168.0.0/16 -n 172.16.0.0/16 -n 10.0.0.0/8"

```



![Pasted image 20240404190359](https://github.com/lm3nitro/Projects/assets/55665256/6d339024-7237-47d7-bb5c-c061fc725e19)


# Running ArpWatch on interface:

systemctl enable arpwatch@ens32
systemctl start arpwatch@ens32


# Stopping ArpWatch on interface:

systemctl disable arpwatch@ens32
systemctl stop arpwatch@ens32


Logs:

![Pasted image 20240404191324](https://github.com/lm3nitro/Projects/assets/55665256/1936dc2a-a171-46bd-826b-66c6bc17f41f)




Checking the status:

![Pasted image 20240404190310](https://github.com/lm3nitro/Projects/assets/55665256/20f8a4ca-b43b-4066-8549-55b310b6df2d)




ArpWatch logs:



sudo cat /var/log/syslog | grep arpwatch | tail


![Pasted image 20240404190612](https://github.com/lm3nitro/Projects/assets/55665256/59664832-0848-4718-8233-0fbac2cee537)

sudo tail -f /var/log/syslog | grep arpwatch
![Pasted image 20240404182600](https://github.com/lm3nitro/Projects/assets/55665256/0071107c-66dd-4365-b78b-cdc3dd8a4f7d)


# Detecting ARP poisoning attacks with ArpWatch:

![Pasted image 20240404185855](https://github.com/lm3nitro/Projects/assets/55665256/4e039006-6b3c-49cc-a317-bdc7ad3121c2)

# Responding To ARP Attacks

Upon identifying any of these ARP-related anomalies, we might question the suitable course of action to counter these threats. Here are a couple of possibilities:

Tracing and Identification: First and foremost, the attacker's machine is a physical entity located somewhere. If we manage to locate it, we could potentially halt its activities. On occasions, we might discover that the machine orchestrating the attack is itself compromised and under remote control.

Containment: To stymie any further exfiltration of information by the attacker, we might contemplate disconnecting or isolating the impacted area at the switch or router level. This action could effectively terminate a DoS or MITM attack at its source.

Link layer attacks often fly under the radar. While they may seem insignificant to identify and investigate, their detection could be pivotal in preventing the exfiltration of data from higher layers of the OSI model.
