# Standalone SiLK and TAP

SiLK (System for Internet-Level Knowledge) suite, is a collection of open-source tools designed to help network security professionals collect, store, and analyze large-scale network flow data in an efficient and flexible way. SiLK is particularly useful for monitoring network traffic, detecting anomalies, and identifying malicious activity such as distributed denial-of-service (DDoS) attacks, scanning attempts, or data exfiltration. 

### Scope:

I will be configuring a standalone sensor configuration, where the server will have both YAF and SILK installed and configured. The server interface will be connected to a tap that is listening to all the traffic incoming and outgoing from the network. I will then analyze NetFlow and using SiLK to run different queries. The primary objective is to develop a thorough understanding of network flow data and its implications for security and performance. 

### Physical Topology:

![Pasted image 20240526140924](https://github.com/lm3nitro/Projects/assets/55665256/e1c080da-41ea-45d7-9779-c22beca64c26)

### Tools and Technology:

Network Tap, SiLK, Cisco Router & Switch, and Ubuntu

![Pasted image 20240526141605](https://github.com/lm3nitro/Projects/assets/55665256/22c72896-4437-45a0-acd0-5476eee78b30)

![Pasted image 20240526142127](https://github.com/lm3nitro/Projects/assets/55665256/b13665c3-fcbb-48b5-8359-3f5996676c3c)

![Pasted image 20240526142532](https://github.com/lm3nitro/Projects/assets/55665256/76397c66-1cee-4b64-81c8-40665dacb770)

## Installing Prerequisites

To get started I began by installing the needed prerequisites:

```
apt install libglib2.0-dev liblzo2-dev zlib1g-dev libgnutls28-dev libpcap-dev python3-dev
apt install libmaxminddb-dev
apt install apt-get install build-essential
apt install gcc
```
## Downloading Software

Next, I downloaded SiLK:

```
cd /tmp
wget https://tools.netsa.cert.org/releases/silk-3.19.1.tar.gz
```

![Pasted image 20240526104427](https://github.com/lm3nitro/Projects/assets/55665256/6b5983ce-df2b-4eb1-b3d2-2213e6c237e4)

I also downloaded libfixbuf and YAF

```
wget https://tools.netsa.cert.org/releases/libfixbuf-2.4.1.tar.gz
```

![Pasted image 20240526104510](https://github.com/lm3nitro/Projects/assets/55665256/23bbf04b-9a53-4db7-9782-910e0d6461d8)

YAF (Yet Another Flow) is a tool that works with network flow data, specifically designed to analyze and visualize NetFlow data. I proceeded to donwload YAF:

```
wget https://tools.netsa.cert.org/releases/yaf-2.12.2.tar.gz
```

![Pasted image 20240526104552](https://github.com/lm3nitro/Projects/assets/55665256/a4e0f212-7ccd-41ff-a57f-9687704a1f44)

## Installing libfixbuf

libfixbuf is designed for handling the collection, processing, and analysis of network flow data. Once downlaoded, I proceed to install it:

```
cd /tmp
tar -zxf /tmp/libfixbuf-2.4.1.tar.gz
cd libfixbuf-2.4.1
./configure  --prefix=/usr/local  --enable-silent-rules
make
make install
```

## Installing SiLK

I then installed SiLK:

```
cd /tmp
tar -zxf /tmp/silk-3.19.1.tar.gz
cd silk-3.19.1
./configure  --prefix=/usr/local  --enable-silent-rules  --enable-data-rootdir=/var/silk/data  --enable-ipv6  --enable-ipset-compatibility=3.14.0  --enable-output-compression --with-python --with-python-prefix
```

Now that the code is configured, I built and installed SiLK. 

```
make
make install
```

## Installing YAF

Previously I downloaded YAF, now I needed to install it:

```
cd /tmp
tar -zxf /tmp/yaf-2.12.2.tar.gz
cd yaf-2.12.2
./configure  --prefix=/usr/local  --enable-silent-rules  --enable-applabel --enable-metadata --enable-plugins
make
make install
```

I then manually copied the YAF start-up script into place:

```
cp /tmp/yaf-2.12.2/etc/init.d/yaf /etc/init.d/yaf
chmod a+x /etc/init.d/yaf
```

## Update Dynamic Linker

Once everything was ddownloaded and installed, I updated the cache of the dynamic linker. If you do not, it may be necessary to set the LD_LIBRARY_PATH environment variable to /usr/local/lib when you use SiLK or YAF. Typically an entry for /usr/local/lib will already exist in the /etc/ld.so.conf.d/ directory. To confirm, I used the following command: 

```
grep local /etc/ld.so.conf.d/*
```

You should see something like this : /etc/ld.so.conf.d/libc.conf:/usr/local/lib

![Pasted image 20240526105723](https://github.com/lm3nitro/Projects/assets/55665256/240ac738-ca61-4c49-97f3-f909470121ab)

To update the cache in this case, I ran the ldconfig program. 

## Configuring SiLK:

I then moved on to configuring SiLK:

```
mkdir -p /var/silk/data
chmod go+rx /var/silk /var/silk/data
cp /usr/local/share/silk/twoway-silk.conf /var/silk/data/silk.conf
```

Part of the SiLK configuration is the rwflowpack, it is specifically designed for processing flow data, like NetFlow and IPFIX, to facilitate network analysis. TO configure rwflowpack I did the following:

1. Created the sensors.conf file that is used by rwflowpack for collecting data from yaf.
   
```
probe S0 ipfix
 listen-on-port 18001
 protocol tcp
 listen-as-host 127.0.0.1
end probe

group my-network
 ipblocks 192.168.1.0/24  # address of ethernet interface. CHANGE THIS.
 ipblocks 10.0.0.0/8      # other blocks considered internal. OPTIONAL.
end group

sensor S0
 ipfix-probes S0
 internal-ipblocks @my-network
 external-ipblocks remainder
end sensor
```

![Pasted image 20240526110223](https://github.com/lm3nitro/Projects/assets/55665256/2c0ff429-a276-4098-abd4-33e347328974)

2. I moved this file into place:

```
mv sensors.conf /var/silk/sensors.conf
```

3. SiLK comes with traditional "init script" start-up files, with two files per daemon: One file is invoked by the system to start the daemon, and the other contains configuration settings used by the first. Copy rwflowpack's start-up script to /etc/init.d/rwflowpack, and copy the configuration settings script to /usr/local/etc/rwflowpack.conf:

```
cd /usr/local
cp share/silk/etc/init.d/rwflowpack /etc/init.d/rwflowpack
cp share/silk/etc/rwflowpack.conf /usr/local/etc/rwflowpack.conf
```

Open the /usr/local/etc/rwflowpack.conf file in an editor and change these variables to the values shown here:

```
ENABLED=1
statedirectory=/var/silk
SENSOR_CONFIG=/var/silk/sensors.conf
ARCHIVE_DIR=  # empty
LOG_TYPE=legacy
LOG_DIR=/var/log
PID_DIR=/var/run
```

4. Start rwflowpack (a message about "contains no runlevels, aborting" is non-fatal)

```
systemctl enable rwflowpack
```

![Pasted image 20240526110813](https://github.com/lm3nitro/Projects/assets/55665256/f90377fd-c1f7-46d0-9270-771201d8227f)

```
systemctl start rwflowpack.service
systemctl status rwflowpack
```

![Pasted image 20240526110935](https://github.com/lm3nitro/Projects/assets/55665256/d1e948cc-0b2b-4703-b9d6-3ee7800a8f1e)

```
systemctl stop rwflowpack
```

## YAF:

Like SiLK, YAF has two "init script" start-up files. Edit the YAF start-up configuration file, /usr/local/etc/yaf.conf, setting these values:

> [!IMPORTANT]  
> Make sure the interface (enp0s3) matches the interface on which you want to capture.

![Pasted image 20240526111217](https://github.com/lm3nitro/Projects/assets/55665256/6fb26b7e-3300-4a7e-a304-9bee2ef140f2)

```
ENABLED=1
YAF_CAP_IF=enp0s3      # Ensure this is correct for your machine
YAF_IPFIX_PORT=18001   # Must match value in sensors.conf
YAF_EXTRAFLAGS="--silk --applabel --max-payload=512"
```

![Pasted image 20240526111429](https://github.com/lm3nitro/Projects/assets/55665256/d9bda39f-acc6-4dd2-a626-f76c2b337717)

As the root user, start yaf (a message about "contains no runlevels, aborting" is non-fatal):

```
systemctl enable yaf
systemctl stop yaf
systemctl start yaf.service
systemctl status yaf
```

![Pasted image 20240526111621](https://github.com/lm3nitro/Projects/assets/55665256/21ca295a-b66c-415a-bd0c-57dac3c09a2a)

You can set multiple options in one "ethtool" command, but this can be problematic if your card doesn't support all of the settings.  To avoid this, you could invoke ethtool for each option like this:

```
ethtool -K ens33 rx off
ethtool -K ens33 tx off
ethtool -K ens33 sg off
ethtool -K ens33 tso off
ethtool -K ens33 ufo off
ethtool -K ens33 gso off
ethtool -K ens33 gro off
ethtool -K ens33 lro off
```

![Pasted image 20240526113103](https://github.com/lm3nitro/Projects/assets/55665256/94d22806-c92a-4f0c-847f-2811c94b3505)

I could simply wrap the ethtool command in a for-loop like this:

```
for i in rx tx sg tso ufo gso gro lro; do ethtool -K ens33 $i off; done
```

![Pasted image 20240526113122](https://github.com/lm3nitro/Projects/assets/55665256/68dfe07b-2ada-4c5d-98e7-63809600f55a)

Next, I verified the sniffing interface. 

```
ethtool -K ens33
```

![Pasted image 20240526113001](https://github.com/lm3nitro/Projects/assets/55665256/b81d2fda-c63b-4e91-9f0b-55c792d2d60f)

After verifying, I enabled promiscuous mode:

```
ifconfig ens33 promisc
```

![Pasted image 20240526132821](https://github.com/lm3nitro/Projects/assets/55665256/8f19eb93-ed97-44f4-b844-9e78af29f9de)

Monitoring the sniffing interface for activity with Nload:

![Pasted image 20240526143326](https://github.com/lm3nitro/Projects/assets/55665256/407477fe-692d-4f6e-9ea7-76901e07a038)

I also checked the service status:

```
systemctl status rwflowpack.service
```

![Pasted image 20240526121551](https://github.com/lm3nitro/Projects/assets/55665256/3636efa3-79d5-438d-868b-b0dd0b5efc93)

```
systemctl status yaf.service
```

![Pasted image 20240526121522](https://github.com/lm3nitro/Projects/assets/55665256/447298b0-210d-4fdf-808f-3601b84c56dd)

## Testing:

I ran the following query to test:

```
/usr/local/bin/rwfilter --sensor=S0 --type=all --all=stdout | rwcut --tail-recs=10
```

![Pasted image 20240526112539](https://github.com/lm3nitro/Projects/assets/55665256/4286d25b-d150-4dcb-816e-e9efaf677ce3)

Below is the location for the SiLK Data Store:

```
/var/silk
```

![Pasted image 20240526121405](https://github.com/lm3nitro/Projects/assets/55665256/b9244e56-2926-4ab8-9b98-0218f33cd8ed)

## Logs

Now that everything is installed and running and I was able to verify that it is working and information was present running the query above, I looked into the individual logs:

1. rwflowpack logs :

```
tail -f  rwflowpack-20240526.log
```

![Pasted image 20240526130359](https://github.com/lm3nitro/Projects/assets/55665256/2a826c1a-edb0-4d00-a596-b3f6717742a3)

2. Yaf logs :

```
tail -f yaf.log
```

![Pasted image 20240526130450](https://github.com/lm3nitro/Projects/assets/55665256/4ee655df-52d8-466a-87f5-b752cb3effab)

## Traffic Type and Repo

rwsiteinfo is a tool within SiLK that provides information about the sites (IP address ranges) contained within a SiLK data store. It helps users analyze and gain insights into the geographical and organizational characteristics of network traffic based on IP addresses. To take a look at the tool I used the command below:

```
rwsiteinfo --sensor=S0 --fields=type,repo-file-count,repo-start-date,repo-end-date
```

![Pasted image 20240526114425](https://github.com/lm3nitro/Projects/assets/55665256/ce884fa8-0cea-411b-8821-0661751aca8f)

> [!NOTE]  
> When a SiLK installation is built to use the local time zone (to determine if this is the case, check the Timezone support value in the output from the --version switch on most SiLK applications), the value of the TZ environment variable determines the time zone in which timestamps are displayed and parsed. If the TZ environment variable is not set, the default time zone is used. Setting TZ to 0 or to the empty string causes timestamps to be displayed in and parsed as UTC. The value of the TZ environment variable is ignored when the SiLK installation uses UTC unless the user requests use of the local time zone via a tool's --timestamp-format switch. For system information on the TZ variable:
> ![Pasted image 20240526113743](https://github.com/lm3nitro/Projects/assets/55665256/7ec06d08-12e4-40a1-8e19-6143359332bd)

## Queries

Once everything what set, I wanted to analyze NetFlow data. Below are some of the queries that I ran to get an overview of what was going on in the network. 

1. This statistic shows the percentage of the records generated by source IP:

```
rwfilter --proto=0-255 --type=all --pass=stdout | rwstats --top --flows --count=20 --fields=sip
```

![Pasted image 20240526152555](https://github.com/lm3nitro/Projects/assets/55665256/710c07f5-4eb8-4ae8-8d0a-b6f3aa87dae1)

2. This shows 20 records that will show the source IP, destination IP, port, protocol, bytes, and the number of packets

```
rwfilter --type=all --proto=0-255 --pass=stdout | rwcut --fields=sip,dip,sport,dport,proto,bytes,packets --num-recs=20
```

![Pasted image 20240526160056](https://github.com/lm3nitro/Projects/assets/55665256/a7666989-3c37-479e-b793-2641907b745c)

3. This is looking for the top 20 source-destination pairs based on total bytes transferred. In this traffic coming I am the destination:

```
rwfilter --type=all --proto=0-255 --pass=stdout | rwuniq --fields=sip,dip --values=bytes,packets --sort-output | head -n 20
```

![Pasted image 20240526160753](https://github.com/lm3nitro/Projects/assets/55665256/1d774611-844a-46ed-b0fa-1d2ea04a4c70)

4. Identifying long connections that have lasted more than 28 minutes:

```
rwfilter --type=all --duration=1700- --pass=stdout | rwcut --num-recs=20
```

![Pasted image 20240526164224](https://github.com/lm3nitro/Projects/assets/55665256/97206c00-76bc-4187-8389-a5f211cc075c)

5. Shows large data transfers:

```
rwfilter --type=all --bytes=50000000- --pass=stdout | rwcut --num-recs=20
```

![Pasted image 20240526165449](https://github.com/lm3nitro/Projects/assets/55665256/faf08c07-29cb-4d80-ab6f-dd524b2354f7)

### Summary:

Using SiLK allows for a more comprehensive visibility. This visibility is crucial for understanding bandwidth usage, identifying top talkers, and monitoring application performance, which can help in capacity planning and optimizing network resources. By analyzing flow data, SiLK can help detect unusual activity that may indicate security threats, such as DDoS attacks or data exfiltration attempts. It provides insights into communication patterns that might go unnoticed by traditional security measures, thus enabling quicker response times to potential incidents.

By installing, configuring, and using SiLK, I was able to gain practical experience with the entire flow analysis process—from configuring network devices to export NetFlow data to using SiLK tools for filtering, sorting, and querying. One of that key objectives that I got from doing this project was learning how to spot unusual patterns in network traffic that could indicate security issues. This includes understanding what constitutes normal behavior for a given network and recognizing deviations that may suggest a threat.

