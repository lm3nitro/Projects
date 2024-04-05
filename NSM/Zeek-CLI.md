## Zeek Cut Install

This is when I decided to install Zeek as a command line tool. The zeek tool  extracts specific fields or data from Zeek logs, allowing for precise data extraction and analysis. Before I can install Zeek, I need to ensure that I have the required dependencies.

![Pasted image 20240328175144](https://github.com/lm3nitro/Projects/assets/55665256/a26b5a58-04a7-457a-8a49-3de6d9e06ab6)

>##### Install Zeek Dependencies 
```
sudo apt-get install python3-git python3-semantic-version
```
![Pasted image 20240328180121](https://github.com/lm3nitro/Projects/assets/55665256/a2b8a55b-60bb-46c3-b27b-bee307b96302)

```
sudo apt-get install cmake make gcc g++ flex libfl-dev bison libpcap-dev libssl-dev python3 python3-dev swig zlib1g-dev
```
![Pasted image 20240328175953](https://github.com/lm3nitro/Projects/assets/55665256/f8e9ca90-36a5-488a-9448-b089fb0f5651)

>##### Retrieving the Sources
```
git clone --recurse-submodules https://github.com/zeek/zeek
```
![Pasted image 20240328175850](https://github.com/lm3nitro/Projects/assets/55665256/4b3770f6-f7f7-4315-815c-30f530353c1f)

>##### Configuring and Building
```
./configure
```
![Pasted image 20240328180338](https://github.com/lm3nitro/Projects/assets/55665256/9907fe4a-dee7-4bf5-979f-59399e95430a)

After configuring, I ran the make command to build Zeek:
```
make
```
![Pasted image 20240328180401](https://github.com/lm3nitro/Projects/assets/55665256/cbadbe06-10d0-4b3f-9858-84f79194f12a)

Lastly, I used the 'make install' to install Zeek to the specified prefix directory (in this case, /usr/local/zeek):
```
make install
```
Output:

![Pasted image 20240328193130](https://github.com/lm3nitro/Projects/assets/55665256/823c800a-a775-40ac-9f39-c62c2933639e)

Once that is complete, I used the following command to verify the version of Zeek that is installed:
```
./zeek -h
```
![Pasted image 20240328193023](https://github.com/lm3nitro/Projects/assets/55665256/16d2314f-a2a5-45fd-b823-8ed79f36d6e6)

The following modifications to Zeek's configuration are optional:

>##### Change Zeek Cut logs to output in JSON format persistently, you can use Zeek's built-in functionality to output logs in JSON format directly.
I personally like to output the logs in json format, I feel that it improves the readability and accessibility of the logs when seen in Splunk. To do so, I edited the zeekctl.cfg and added the following:
```
@load frameworks/files/extract-all-files  
redef ignore_checksums=T;  
redef LogAscii::use_json=T;
```
Offloading in Zeek means shifting some of its work, like heavy calculations or storage tasks, to other systems or specialized hardware. This helps Zeek run more efficiently by letting it focus on important security monitoring and analysis.

For offloading:
```
for i in rx tx sg tso ufo gso gro lro; do
    ethtool -K enp0s8 $i off
done
```
After Zeek Cut is installed, which is by default to the target directory /usr/local/zeek/bin. Execute the following command to add the _/opt/zeek/bin_ directory to the system **PATH** via _~/.bashrc_ file.
```
echo "export PATH=$PATH:/usr/local/zeek/bin" >> ~/.bashrc
```
Next, reload the _~/.bashrc_ file and check the system **PATH** variable using the following command. You should see the _/opt/zeek/bin_ directory within the system PATH.
```
source ~/.bashrc  
echo $PATH
```
To read pcaps files with zeek and have the output in json format:

```
zeek -C -r ../tm1t.pcap LogAscii::use_json=T
```
More information about Zeek log formats can be found in the link below:

https://github.com/zeek/zeek-docs/blob/master/log-formats.rst

Setting the correct time zone in Zeek is important for accurate log timestamps and facilitating correlation with other logs.

>##### Configuration of time zone:
```
timedatectl list-timezones
```
```
timedatectl set-timezone UTC
```

We can now see how the zeek conn.log in json format in Splunk:

![Pasted image 20240328212902](https://github.com/lm3nitro/Projects/assets/55665256/0584be89-ba59-4aec-ae76-69e872eed795)

Here we can also see the ja3 hashes json format: 

![Pasted image 20240331164157](https://github.com/lm3nitro/Projects/assets/55665256/8188f8e7-9a8e-4814-8225-ccffaa01d00d)

JA3 hashes are very important in threat hunting because they provide a way to fingerprint TLS connections. This allows security analysts to detect and analyze potential threats such as malicious traffic, anomalous behavior, or suspicious SSL/TLS configurations. 
