# Zeek (CLI) Install

<img width="547" alt="Screenshot 2024-11-12 at 9 32 23 PM" src="https://github.com/user-attachments/assets/bc7fe8bd-e92e-4ca6-8151-c52b0ec60cc4">

Zeek (formerly known as Bro) is a powerful network analysis tool primarily used for monitoring and analyzing network traffic in real-time. It's a highly extensible, open-source framework designed for network security monitoring, incident response, and traffic analysis. 

### Scope:

In this lab, I will install Zeek as a command-line tool to enhance network monitoring and security analysis. Zeek is designed to capture and process specific data fields from its logs, enabling precise extraction and analysis of network traffic. Once Zeek is installed, I will configure it to generate logs and monitor the extracted data in Splunk, allowing for real-time analysis and visualization of network traffic and security events.

### Tools and Technology
Linux, Zeek, and Splunk

## Getting Started: 

First, I went to the main page to verify the latest version. At the time of this writing the latest version was 6.0.3:

![Pasted image 20240328175144](https://github.com/lm3nitro/Projects/assets/55665256/a26b5a58-04a7-457a-8a49-3de6d9e06ab6)

Next, I proceeded to install all dependencies:

```
sudo apt-get install python3-git python3-semantic-version
```

![Pasted image 20240328180121](https://github.com/lm3nitro/Projects/assets/55665256/a2b8a55b-60bb-46c3-b27b-bee307b96302)

More dependencies:

```
sudo apt-get install cmake make gcc g++ flex libfl-dev bison libpcap-dev libssl-dev python3 python3-dev swig zlib1g-dev
```

![Pasted image 20240328175953](https://github.com/lm3nitro/Projects/assets/55665256/f8e9ca90-36a5-488a-9448-b089fb0f5651)

Next, I created a directory for Zeek, and then cloned not only the main repository but all the submodules as well:

```
git clone --recurse-submodules https://github.com/zeek/zeek
```

![Pasted image 20240328175850](https://github.com/lm3nitro/Projects/assets/55665256/4b3770f6-f7f7-4315-815c-30f530353c1f)

I then used the following command `./configire`, this will check and ensure that all depencies are in place prior to installing:

```
./configure
```

![Pasted image 20240328180338](https://github.com/lm3nitro/Projects/assets/55665256/9907fe4a-dee7-4bf5-979f-59399e95430a)

Next, I ran the `make` and `make install` commands to build and install Zeek:

```
make
```

![Pasted image 20240328180401](https://github.com/lm3nitro/Projects/assets/55665256/cbadbe06-10d0-4b3f-9858-84f79194f12a)


```
make install
```

![Pasted image 20240328193130](https://github.com/lm3nitro/Projects/assets/55665256/823c800a-a775-40ac-9f39-c62c2933639e)

Once that completed, I used the following command to verify the version of Zeek that is installed:

```
./zeek -h
```

![Pasted image 20240328193023](https://github.com/lm3nitro/Projects/assets/55665256/16d2314f-a2a5-45fd-b823-8ed79f36d6e6)

At this time version 7 was isntalled. 
__________________________________________________________________________________________________________________________________________________
## Optional Configuration/Modifications:

The following changes to configuration are optional:

Option 1: Change Zeek Cut logs to output in JSON format persistently: One option is to use Zeek's built-in functionality to output logs in JSON format directly.I personally like to output the logs in json format, I feel that it improves the readability and accessibility of the logs when seen in Splunk. To do so, I edited the zeekctl.cfg and added the following:

```
@load frameworks/files/extract-all-files  
redef ignore_checksums=T;  
redef LogAscii::use_json=T;
```

Option 2: Offloading: Offloading in Zeek means shifting some of its work, like heavy calculations or storage tasks, to other systems or specialized hardware. This helps Zeek run more efficiently by letting it focus on important security monitoring and analysis.

For offloading I used the following:

```
for i in rx tx sg tso ufo gso gro lro; do
    ethtool -K enp0s8 $i off
done
```
___________________________________________________________________________________________________________________________________________________

When Zeek cut is initially installed, it gets installed in the default directory /usr/local/zeek/bin. I executed the following command to add the _/opt/zeek/bin_ directory to the system **PATH** via _~/.bashrc_ file.

```
echo "export PATH=$PATH:/usr/local/zeek/bin" >> ~/.bashrc
```

Next, I reloaded the _~/.bashrc_ file and checked the system **PATH** variable using the following command. 

```
source ~/.bashrc  
echo $PATH
```

In order to read pcaps files with zeek and have the output in json format, I used the following command:

```
zeek -C -r ../tm1t.pcap LogAscii::use_json=T
```

More information about Zeek log formats can be found in the following link:

```
https://github.com/zeek/zeek-docs/blob/master/log-formats.rst
```

## Time Configuration

Setting the correct time zone in Zeek is important for accurate log timestamps and facilitating correlation with other logs. To ensure the correct time zone was in place, I did the following: 

```
timedatectl list-timezones
```
```
timedatectl set-timezone UTC
```

## Splunk

Once Zeek was installed, and running, I then configured it to send the logs to Splunk. I then cerified that the logs were getting received by Splunk. Below is the Zeek conn.log in json format in Splunk:

![Pasted image 20240328212902](https://github.com/lm3nitro/Projects/assets/55665256/0584be89-ba59-4aec-ae76-69e872eed795)

I can also see the ja3 hashes: 

![Pasted image 20240331164157](https://github.com/lm3nitro/Projects/assets/55665256/8188f8e7-9a8e-4814-8225-ccffaa01d00d)

JA3 hashes are very important in threat hunting because they provide a way to fingerprint TLS connections. This allows security analysts to detect and analyze potential threats such as malicious traffic, anomalous behavior, or suspicious SSL/TLS configurations. 

### Summary

