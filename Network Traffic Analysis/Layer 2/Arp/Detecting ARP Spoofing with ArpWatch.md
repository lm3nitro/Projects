# Detecting ARP Spoofing with ArpWatch

Arpwatch is an open source software that helps to monitor ethernet traffic activity (like Changing IP and MAC Addresses) on your network and maintains a database of ethernet/ip address pairings. It also creates a record of these pairings along with timestamps, helping you see when these changes occurred. Arpwatch can also send email reports whenever a pairing is added or modified.

### Scope:

In this exercise, I will install and configure Arpwatch in my lab environment and then simulate an ARP attack to test the effectiveness of its detection capabilities. This will allow me to evaluate how well Arpwatch identifies and alerts on suspicious ARP activity.

### Tools and Technology:
Linux and Arpwatch

## Installing and Configuring ARPWatch

To start, I installed ARPWatch:

```
apt install arpwatch
```

![Pasted image 20240404191910](https://github.com/lm3nitro/Projects/assets/55665256/a42e6239-19ba-429c-8a94-3bf07ab291ba)

Once I got that installed, I needed to get it configured. To do this, I went to the `/etc/arpwatch/` directory and created a file named `enp32.iface`. 

> [!NOTE]  
> This file was done in sh syntax. In the file called `ens32.iface`, which contains variable assignments, the following variables can be used to influence the invocation for that specific interface only:
> + ARGS: overwrite the ARGS from /etc/default/arpwatch
> + PCAP_FILTER: overwrite (or set) the pcap filter
> + IFACE_ARGS: additional options to be passed to arpwatch
> 
> Options:
> + The -m option is used to specify the e-mail address to which reports will be sent. By default, reports are sent to root on the local machine.
> + The -n flag specifies additional local networks. This can be useful to avoid 'bogon' warnings when there is more than one network running on the same wire. If the optional width/mask is not specified, the default netmask for the network's class is used.
> + The -N flag disables reporting any bogons.
> + The -p flag disables promiscuous operation. ARP broadcasts get through hubs without having the interface in promiscuous mode, while saving considerable resources that would be wasted on processing gigabytes of non-broadcast traffic.

Example file configuration:

```
INTERFACES="ens32"
ARGS="-N -p"
IFACE_ARGS="-m erick.gomez@lm3nitro.com -n 192.168.0.0/16 -n 172.16.0.0/16 -n 10.0.0.0/8"
```
![Pasted image 20240404190359](https://github.com/lm3nitro/Projects/assets/55665256/6d339024-7237-47d7-bb5c-c061fc725e19)

## Running ArpWatch

Now that Arpwatch is installed and the configuration is in place, I can have ARPWatch running on a specific inferace, in my case it is interface `ens32`:

```
systemctl enable arpwatch@ens32
systemctl start arpwatch@ens32
```

These are the commands to stop ARPWatch:

```
systemctl disable arpwatch@ens32
systemctl stop arpwatch@ens32
```

Here is a view of the Logs:

![Pasted image 20240404191324](https://github.com/lm3nitro/Projects/assets/55665256/1936dc2a-a171-46bd-826b-66c6bc17f41f)

Checking the status:

![Pasted image 20240404190310](https://github.com/lm3nitro/Projects/assets/55665256/20f8a4ca-b43b-4066-8549-55b310b6df2d)


## Analysis

After I verified that the service was up and running, I then simulated an ARP attack. Below are the Arpwatch logs generated from the activity:

```
sudo cat /var/log/syslog | grep arpwatch | tail
```

![Pasted image 20240404190612](https://github.com/lm3nitro/Projects/assets/55665256/59664832-0848-4718-8233-0fbac2cee537)

```
sudo tail -f /var/log/syslog | grep arpwatch
```

![Pasted image 20240404182600](https://github.com/lm3nitro/Projects/assets/55665256/0071107c-66dd-4365-b78b-cdc3dd8a4f7d)

Arpwatch can detect ARP poisoning by tracking changes in MAC-to-IP address mappings on the network. It compares these changes to a baseline of normal ARP traffic. This allows for prompt investigation and mitigation of suspicious activity. Below is another look at the mismatch:

![Pasted image 20240404185855](https://github.com/lm3nitro/Projects/assets/55665256/4e039006-6b3c-49cc-a317-bdc7ad3121c2)

### Summary: 

In this exercise, I was able to install and configure Arpwatch and then test its detection capabilities on suspicious ARP activity. I also gained hands-on experience in monitoring ARP traffic for potential spoofing or poisoning attacks. Simulating an ARP attack allowed me to observe how Arpwatch detects changes in IP-to-MAC mappings and triggers alerts when suspicious activity occurs. This exercise helped me understand the importance of ARP monitoring in network security and how tools like Arpwatch can help detect and mitigate ARP-based threats. Overall, monitoring the network for ARP-based threats is important because ARP spoofing or poisoning can allow attackers to intercept or redirect network traffic, leading to security breaches like man-in-the-middle attacks, network outages, etc.
