# SiLK 

SiLK is a suite of network traffic analysis tools designed for collecting, storing, and analyzing NetFlow and IPFIX data. It helps to monitor network traffic, detect anomalies, and improve security by analyzing flow data. It is highly scalable, capable of handling large volumes of traffic data, and is commonly used for security monitoring, troubleshooting, and performance analysis in both small and large networks. SiLK can also be used as a PCAP analysis tool; it can be installed on a desktop or run as a NetFlow collector.

### Scope:

In this project I will be installing and configuring SiLK. Once installed, I will be analyzing a pcap file. I will employ rwp2yaf2silk to convert the pcap file into NetFlow-compatible data, which is then ingested by Silk for further processing. I will then use rwfilter to filter specific traffic based on criteria such as IP addresses, protocols, etc.

### Tools and Technology:
Linux, YAF, and SiLK

Network Topology:
![Pasted image 20240529215556](https://github.com/lm3nitro/Projects/assets/55665256/68d5c662-b838-48f1-b950-066d406b246b)

In this diagram, SiLK is placed in the out-of-band network collecting NetFlow records from the fireall and router on the network. From there the Silk server can be accessed and queries can be made.

## Prerequisites:

Prior to installing SiLK, I first ensured that all the packages were up to date:

```
apt update
```

![Pasted image 20240509154613](https://github.com/lm3nitro/Projects/assets/55665256/9acfbceb-90f3-4a1b-b42e-b969a0bb3c92)

I then upgraded:

```
apt upgrade -y
```

![Pasted image 20240509154647](https://github.com/lm3nitro/Projects/assets/55665256/10b29cc6-09e0-4d07-be35-e50ececb2e30)

Next, I needed to install the needed prerequisites. These are the basic development tools for C compiler required to build SiLK and YAF:

```
apt install build-essential
```

![Pasted image 20240509155257](https://github.com/lm3nitro/Projects/assets/55665256/360a7039-383f-49a0-a193-7e5401615ba9)

I then ran the following command to install the GLib-2, LZO, zlib, GnuTLS, PCAP, and Python development libraries.

```
apt install libglib2.0-dev liblzo2-dev zlib1g-dev libgnutls28-dev libpcap-dev
```

![Pasted image 20240509155511](https://github.com/lm3nitro/Projects/assets/55665256/d3ebeb64-e190-4660-a63c-c51ce46107b1)

```
sudo apt-get install python3-dev
```

![Pasted image 20240509155635](https://github.com/lm3nitro/Projects/assets/55665256/d83daf54-78ed-410b-8244-24ed62daf709)


To use the MaxMind country-code mapping capability of SiLK, I installed the libmaxminddb-dev package:

```
apt install libmaxminddb-dev
```

![Pasted image 20240509155741](https://github.com/lm3nitro/Projects/assets/55665256/f57d8ec4-0d45-48b0-8c92-a5c3776131f8)

## Downloading

Now that the the prerequisites were installed, I tcould install SiLK. I downloaded it into the /tmp directory:

```
wget https://tools.netsa.cert.org/releases/silk-3.19.1.tar.gz
```

![Pasted image 20240509155914](https://github.com/lm3nitro/Projects/assets/55665256/eab177d2-692d-426d-a463-11c9da408017)

SiLK works with IPFIX, for this, libfixbuf is needed. Libfixbuf is an open-source C library designed for handling IPFIX flow data:

```
wget https://tools.netsa.cert.org/releases/libfixbuf-2.4.1.tar.gz
```

![Pasted image 20240509155945](https://github.com/lm3nitro/Projects/assets/55665256/6c18e01e-742e-480a-aa5d-86aa02a2f5cf)

Also YAF:

```
wget https://tools.netsa.cert.org/releases/yaf-2.12.2.tar.gz
```

![Pasted image 20240509160018](https://github.com/lm3nitro/Projects/assets/55665256/286e843d-5b23-4726-9d66-b3a0187ee924)

Files downloded:

![Pasted image 20240509160122](https://github.com/lm3nitro/Projects/assets/55665256/bead7520-17b0-491d-92a2-f1cee78fc8f2)

## Installation:

1. Now that I downloaded what was needed, I proceeded to install:

I started first with libfixbuf. I unpackaged, configured, and installed libfixbuf:

```
tar -xvzf libfixbuf-2.4.1.tar.gz
```

![Pasted image 20240509160251](https://github.com/lm3nitro/Projects/assets/55665256/e1eaf337-50e4-4330-83fe-4485b362f701)

```
./configure --prefix=/usr/local --enable-silent-rules
```

![Pasted image 20240509160455](https://github.com/lm3nitro/Projects/assets/55665256/ef55cde2-4b29-4367-b42e-e55c012c1ff1)

```
make
```

![Pasted image 20240509160550](https://github.com/lm3nitro/Projects/assets/55665256/65a795bd-e22a-4f87-81b6-f378afb6b3e9)

```
make install
```

![Pasted image 20240509160638](https://github.com/lm3nitro/Projects/assets/55665256/cbae0445-c4b5-47ec-9bfe-a0df20b2a67b)


2. Next, I installed SilK. 

```
tar -zxf /tmp/silk-3.19.1.tar.gz
cd silk-3.19.1
./configure --prefix=/usr/local --enable-silent-rules --enable-data-rootdir=/var/silk/data --enable-ipv6 --enable-ipset-compatibility=3.14.0 --enable-output-compression --with-python --with-python-prefix
```

![Pasted image 20240509161229](https://github.com/lm3nitro/Projects/assets/55665256/7302079e-6dd2-4c55-b27b-f1d8bcd35135)

At the time of this writing the latest version was SiLK 3.19.1

![Pasted image 20240509161336](https://github.com/lm3nitro/Projects/assets/55665256/c989c7ee-6e40-4ffb-9eb8-cb120fe36dca)

```
make
```

![Pasted image 20240509161557](https://github.com/lm3nitro/Projects/assets/55665256/4041e747-666b-443a-ba09-c76283b379f2)

```
make install
```

![Pasted image 20240509161711](https://github.com/lm3nitro/Projects/assets/55665256/9d80f3c2-2ca5-4ab5-893e-dacf31b32b8d)

3.Lastly, I needed to install YAF:

```
cd /tmp
tar -zxf /tmp/yaf-2.12.2.tar.gz
cd yaf-2.12.2
./configure --prefix=/usr/local --enable-silent-rules --enable-applabel --enable-metadata --enable-plugins
```

![Pasted image 20240509162034](https://github.com/lm3nitro/Projects/assets/55665256/59102550-051d-4647-9bf2-c215b8b85a48)

Verified the version:

![Pasted image 20240509162110](https://github.com/lm3nitro/Projects/assets/55665256/fb2f61be-8754-4b61-ba36-c1913e04cbeb)

```
make
```

![Pasted image 20240509162131](https://github.com/lm3nitro/Projects/assets/55665256/031e8d0b-fb8d-4780-aeba-560415fa4e36)

```
make install
```

![Pasted image 20240509162309](https://github.com/lm3nitro/Projects/assets/55665256/2ba4607f-3acd-47fe-9ca9-198741917833)

I then copied the YAF start-up script into place:

```
cp /tmp/yaf-2.12.2/etc/init.d/yaf /etc/init.d/yaf
chmod a+x /etc/init.d/yaf
```

![Pasted image 20240509162405](https://github.com/lm3nitro/Projects/assets/55665256/49594c4f-1153-47f6-b938-2ef0cb269580)

Once everything was installed, I then needed to update the cache of the dynamic linker. 

```
grep local /etc/ld.so.conf.d/*
```

![Pasted image 20240509162541](https://github.com/lm3nitro/Projects/assets/55665256/e6393627-8562-4ffa-8f7c-e9dfddab527c)

To update the cache in this case, I ran the ldconfig program. 

> [!NOTE]  
>If the host  does not have such an entry another option is to create a file named silk.conf containing the following line that specifies the library directory for SiLK and YAF:
>
> /usr/local/lib
>
> Next, copy the file into the /etc/ld.so.conf.d directory and run ldconfig.
> ```
> mv silk.conf /etc/ld.so.conf.d/.
> ldconfig
> ```

## Analysis:

All installed tools are in /usr/local/bin. I have highlighted that ones I will be using:

![Pasted image 20240509183609](https://github.com/lm3nitro/Projects/assets/55665256/cd259460-063b-4cd7-a713-58c302a0059f)

Next, I captured traffic in order to generate a pcap that will be used for analysis:

![Pasted image 20240510133659](https://github.com/lm3nitro/Projects/assets/55665256/5d35fc99-aaa7-431e-83ff-5b7c1d3ee98b)

I then used rwp2yaf2silk to convert the pcap data to SiLK Flow Records:

![Pasted image 20240510160856](https://github.com/lm3nitro/Projects/assets/55665256/432ec0d7-5aa7-4d66-894d-fb4a9938b31f)

```
rwp2yaf2silk --in=lm3nitro_flow_to_silk.cap --out=lm3nitro_flow_to_silk.silk
```

![Pasted image 20240510133957](https://github.com/lm3nitro/Projects/assets/55665256/3af1d7f8-7e34-4ce7-ac04-eb53e7034648)

Once converted, I was then able to analyze the flow data:

```
rwfilter lm3nitro_flow_to_silk.silk --proto=0-255 --pass=stdout --max-pass=2 | rwcut -f 1-10
```

![Pasted image 20240510134411](https://github.com/lm3nitro/Projects/assets/55665256/ad2318f0-06c1-468c-8a83-5469c92dcb86)

Below are some of the queries I used to analyze the traffic:

1. Taking a look at top 20 talkers using rwstats:

```
rwstats --fields=sip --count=20 lm3nitro_flow_to_silk.silk
```

![Pasted image 20240510134607](https://github.com/lm3nitro/Projects/assets/55665256/aea77a81-6891-445b-b022-64acf3171107)

2. Filtering all the connections for a single IP:

```
rwfilter --dcidr=151.101.1.140 --pass=stdout lm3nitro_flow_to_silk.silk | rwcut --fields=sip,sport,dip,dport
```

![Pasted image 20240510135651](https://github.com/lm3nitro/Projects/assets/55665256/8052ff31-8a82-441e-ab2f-476551dc01e0)

3. Many records were returned. I used the following to count just how many:
```
Adding | wc -l 
```

![Pasted image 20240510135820](https://github.com/lm3nitro/Projects/assets/55665256/22f37ba6-3511-46cb-b95d-0fabd8c21c94)

4. Total of unique communication with 151.101.1.140 including ports, byte and packets transfer:

```
rwfilter --dcidr=151.101.1.140 --pass=stdout lm3nitro_flow_to_silk.silk | rwuniq --fields=dport --values=records,bytes,packets --sort-output
```

![Pasted image 20240510140129](https://github.com/lm3nitro/Projects/assets/55665256/311c6869-8e41-4fe7-a97c-ff671d1d531a)

5. Extracted all TCP flows and displayed top 5 destination ports by number of flows:

```
rwfilter lm3nitro_flow_to_silk.silk  --proto=6 --pass=stdout | rwstats --fields dport --count=5 --flows
```

![Pasted image 20240510141124](https://github.com/lm3nitro/Projects/assets/55665256/fe1df959-30c8-4457-99b2-0845240449c8)

> [!TIP]
> #### Additional SiLK Information
> 
> Silk Flow Fields:
> 
> ![Pasted image 20240510155349](https://github.com/lm3nitro/Projects/assets/55665256/e8b085dd-d681-4aba-96d4-8617f0cd2694)
>
> rwfilter format:
> 
> ![Pasted image 20240510155652](https://github.com/lm3nitro/Projects/assets/55665256/96dc9bfb-fb2d-4add-9f8b-fab8145624cf)
>
> Partition Parameters: At least one partitioning parameter required for every rwfilter:
> 
> ![Pasted image 20240510155814](https://github.com/lm3nitro/Projects/assets/55665256/a470d466-e1b3-4106-9f92-5a2533a2a061)
>
> Ouput:
> 
> ![Pasted image 20240510160034](https://github.com/lm3nitro/Projects/assets/55665256/93017d46-78e8-4264-b586-249613d8a1ed)
>
> SiLK commands to process output, rwfilter:
> 
> ![Pasted image 20240510161140](https://github.com/lm3nitro/Projects/assets/55665256/8a828cf4-44b4-4db9-8651-47c0c3e35a1f)

### Summary

In this project, I successfully installed and configured SiLK to transform pcap data into flow-based insights, allowing for more efficient network monitoring and analysis. The primary goal was to streamline the process of analyzing network traffic by converting detailed packet data into NetFlow records, making it easier to assess network performance, identify trends, and detect potential security threats. Through this project, I gained hands-on experience in configuring SiLK for traffic collection, filtering specific flows, and visualizing network behavior.
