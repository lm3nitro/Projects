# Cisco Netflow to SiLK (rwflowpack)

Note: This is a continuation to **Cisco Netflow**

### Scope: 

This is Part 2 of this project. In part 1, I was able to effectively configure and utilize NetFlow on a Cisco router. In part 2 I will be collecting all the NetFlow from the Cisco router and analyzing the traffic using SiLK. SiLK is a suite of tools used for analyzing network traffic, particularly flow data. It allows users to process, analyze, and visualize large volumes of network flow information, which can help in understanding traffic patterns, identifying anomalies, and detecting potential security incidents.

Here is a look at the diagram and topology for better understand of the set-up used for this project:

![Pasted image 20240529222403](https://github.com/lm3nitro/Projects/assets/55665256/169c6648-bc17-4040-8489-ae539a8e8e18)

## NetFlow configuration:

To pick up where I left off, I wanted to verify that everything on the Cisco router was configured and ready to go:

![Pasted image 20240527231718](https://github.com/lm3nitro/Projects/assets/55665256/4dcb6f85-82b6-43a8-a251-5fdc167f29c0)

Information on the router interfaces:

![Pasted image 20240527231745](https://github.com/lm3nitro/Projects/assets/55665256/05fc5d89-5dc2-40fd-a42b-a746af965bda)

In order to export the flow data from the router to my SiLK instance, I ran the commands below
```
ip flow-cache timeout active 1
ip flow-cache timeout inactive 15
ip flow-capture vlan-id
ip flow-capture mac-addresses
ip flow-export version 9 origin-as
ip flow-export destination 172.16.10.10 9996   # rwflowpack IP and port
```

The following commands were used to confirm the configuration:

```
show ip cache flow
show ip flow interface
show ip flow export
show ip flow export template
```

![Pasted image 20240527232533](https://github.com/lm3nitro/Projects/assets/55665256/244e58c6-f0e5-4cc6-9f90-c0a842374be6)

To display information about the NetFlow interfaces

```
show flow interface
```

![Pasted image 20240527224019](https://github.com/lm3nitro/Projects/assets/55665256/4ae5be53-59ae-4de7-bea8-73043831c549)

To display information about the exporters and the statistics:

```
show flow exporter
```

![Pasted image 20240527224056](https://github.com/lm3nitro/Projects/assets/55665256/953ec4c5-ccbc-4a34-a1ac-b6a9a5d52671)

The following command shows a detailed summary of the NetFlow accounting statistics:

```
show ip cache verbose flow
```

![Pasted image 20240527224232](https://github.com/lm3nitro/Projects/assets/55665256/a4a46348-f88d-43ac-823c-8c7d255d3e84)

## SiLK Dynamic Linker

Once everything was set up on the router for NetFlow, I needed to update the cache of the dynamic linker. If you do not update the dynamic linker, it may be necessary to set the LD_LIBRARY_PATH environment variable to /usr/local/lib when you use SiLK or YAF.

Typically an entry for /usr/local/lib will already exist in the /etc/ld.so.conf.d/ directory. To confirm:

```
grep local /etc/ld.so.conf.d/*
/etc/ld.so.conf.d/libc.conf:/usr/local/lib
```

![Pasted image 20240527223036](https://github.com/lm3nitro/Projects/assets/55665256/b2f53a13-f4a0-4cce-b898-9077bee7c899)

To update the cache in this case, run the ldconfig program:

```
sudo ldconfig
```

## Configuring SiLK:

The first step to configuring SiLK is to create the data repository directory (/var/silk/data) and adding the silk.conf file, which defines how your data is stored. Use the default silk.conf file for the twoway site, which is installed at /usr/local/share/silk/twoway-silk.conf. You may edit the sensor descriptions if desired. The default settings cause the SiLK analysis program rwfilter to consider only incoming data unless the user provides the --type or --flowtypes switch. If you want rwfilter to look at both incoming and outgoing data by default, modify the default-types line to include in inweb out outweb. If desired, also add int2int ext2ext.

```
mkdir -p /var/silk/data
chmod go+rx /var/silk /var/silk/data
cp /usr/local/share/silk/twoway-silk.conf /var/silk/data/silk.conf
```
To configure rwflowpack I added the following lines:

```
probe S0 netflow-v9
 listen-on-port 9996
 protocol udp
 accept-from-host 172.16.10.1
end probe

sensor S0
 netflow-v9-probes S0
 internal-ipblocks 172.16.10.0/24
 external-ipblocks remainder
end sensor
```

![Pasted image 20240527215203](https://github.com/lm3nitro/Projects/assets/55665256/5ba4232b-b156-4a38-a1dd-1e388f053557)

Move this file into place.

SiLK comes with traditional "init script" start-up files, with two files per daemon: One file is invoked by the system to start the daemon, and the other contains configuration settings used by the first. Copy rwflowpack's start-up script to /etc/init.d/rwflowpack, and copy the configuration settings script to /usr/local/etc/rwflowpack.conf:

```
mv sensors.conf /var/silk/sensors.conf
```
```
cd /usr/local
cp share/silk/etc/init.d/rwflowpack /etc/init.d/rwflowpack
cp share/silk/etc/rwflowpack.conf /usr/local/etc/rwflowpack.conf
```
Open the `/usr/local/etc/rwflowpack.conf` file in an editor and change these variables to the values shown here:

```
ENABLED=1
statedirectory=/var/silk
SENSOR_CONFIG=/var/silk/sensors.conf
ARCHIVE_DIR=  # empty
LOG_TYPE=legacy
LOG_DIR=/var/log
PID_DIR=/var/run
```

Start rwflowpack (a message about "contains no runlevels, aborting" is non-fatal):

```
systemctl enable rwflowpack
systemctl start rwflowpack.service
systemctl status rwflowpack
```

![Pasted image 20240527222603](https://github.com/lm3nitro/Projects/assets/55665256/1cafd283-bd5f-48ae-b90e-6255fe45f752)

I then verified the listening port for rwflowpack:

![Pasted image 20240527223225](https://github.com/lm3nitro/Projects/assets/55665256/334ca804-f222-454a-aef5-e862d9c49f0e)

Also verified if the NetFlow-v9 was coming from the Router:

![Pasted image 20240527223533](https://github.com/lm3nitro/Projects/assets/55665256/f91eee7f-481b-48ba-a234-770c847c18dc)

I then ran the following query to get data for the current day:

```
/usr/local/bin/rwfilter --sensor=S0 --type=all --all=stdout \
| rwcut --tail-recs=10
```

![Pasted image 20240527222712](https://github.com/lm3nitro/Projects/assets/55665256/e0945dce-dbf9-4e30-8d3f-d9c7bcaf25af)

This was to confirm that the data was being sent and recieved correctly. 

## Data directory:

In the context of SiLK, the silk.conf file is a configuration file that defines various settings, including the availability of plugins like packlogic-twoway.so. If it defines a single class called all, it typically means that this class encompasses all packets or flows processed by the plugin.

![Pasted image 20240527221824](https://github.com/lm3nitro/Projects/assets/55665256/e4ef52fe-a4c6-475c-b18d-1d7910115d5a)

In SiLK, the types assigned to flow records within the `all` class typically depend on the flow's characteristics as it moves between networks. Here’s a general idea of how flow types might be categorized based on direction and state:

1. **in, inicmp, inweb**: Incoming traffic. The traffic is split into multiple types, and these types allow the analysts to query a subset of the flow records depending on their needs. Each incoming flow record is split into the one of incoming types using the following rules:

2. **inweb**: Contains traffic where the protocol is TCP (6) and either the source port or the destination port is one of 80, 443, or 8080

3. **inicmp**: Contains flow records where either the protocol is ICMP (1) or the flow record is IPv6 and the protocol is ICMPV6 (58). By default, the inicmp and outicmp types are not used by the packlogic-twoway.so plug-in.

4. **in**: Contains all other incoming traffic.

5. **out, outicmp, outweb**: Outgoing traffic. The traffic is split among the types using rules similar to those for incoming traffic.

6. **innull**: Blocked incoming traffic

7. **outnull**: Blocked outgoing traffic

8. **ext2ext**: Strictly external traffic

9. **int2int**: Strictly internal traffic

10. **other**: Either traffic from the null network or traffic to or from the unknown network

## Queries 

`rwfilter` is a powerful command-line tool in the SiLK suite designed to filter flow records based on specified criteria. These are some of the queries that I ran in order to analyze the NetFlow data being received:


1. Filtering for dest port 443 in outweb dir. This query helps identify all outgoing web traffic (HTTPS) from the network, allowing for monitoring of secure web communications:

```
rwfilter --dport=443 ow-S0_20240528.00 --pass=stdout | rwcut --field=1-9 --num-recs=10
```

![Pasted image 20240527224953](https://github.com/lm3nitro/Projects/assets/55665256/89e29bca-3d7c-40e9-a3a0-5db2f279989e)

2. Filtering for source port range. This query enables the analysis of traffic originating from a specific range of source ports, helping to identify patterns or potential issues related to specific applications or services:

```
rwfilter --sport=49993-50000 ow-S0_20240528.00 --pass=stdout | rwcut --field=1-9 --num-recs=10
```

![Pasted image 20240527225305](https://github.com/lm3nitro/Projects/assets/55665256/f3f4905f-a3fd-4823-b620-fca37b6c20ca)

3. Printing a summary of traffic stats. This query provides an overview of key traffic metrics, such as total bytes and flow counts, facilitating quick insights into network performance and usage:

```
rwfilter --print-volume-stat ow-S0_20240528.00 --proto=0-255
```

![Pasted image 20240527225604](https://github.com/lm3nitro/Projects/assets/55665256/c7e10da3-24e7-4fd3-898e-4f749f1b4d4d)

4. Viewing All file info. This query allows users to access comprehensive details about flow records, which is essential for in-depth analysis and troubleshooting:

```
rwfileinfo ow-S0_20240528.00
```

![Pasted image 20240527225804](https://github.com/lm3nitro/Projects/assets/55665256/275a2c63-2478-48ba-afd5-1c96e365cb31)

5. `rwcount` interpolating how much traffic is in each bin. Splits each flow proportionally. This query aggregates traffic data into time bins, enabling analysts to visualize traffic patterns and trends over time, with flows accurately represented based on their duration:

```
rwfilter ow-S0_20240528.00 --all=stdout | rwcount --bin-size=500
```

![Pasted image 20240527230430](https://github.com/lm3nitro/Projects/assets/55665256/55e31c86-169e-4b7f-a5f4-c1be4743ce84)

6. Counting number of flows for a particular key. The key is the --field option. This query counts the occurrences of specific attributes (like IP addresses or ports), helping to identify high-traffic sources or destinations for targeted analysis:

```
rwfilter ow-S0_20240528.00 --all=stdout | rwuniq --field=sip,dip,proto | head -10
```

![Pasted image 20240527230923](https://github.com/lm3nitro/Projects/assets/55665256/88611146-9eac-4001-9bc5-edab3db67b70)

### Summary:

In this part 2 of the project, I was able to configure SiLK to receive the NetFlow data from my Cisco router which enabled comprehensive monitoring and analysis of network traffic, providing insights into usage patterns, performance metrics, and potential security threats. I then analyzed the data being ingested into SiLK by using various queries. In addition to supporting the CIA triad, SiLK and NetFlow data also facilitates compliance with regulatory requirements and industry standards, as it provides an audit trail of network activity. 

Ultimately, the purpose of this configuration was to deepen the understanding of network behavior, enhance knowledge of traffic analysis using NetFlow data through hands-on practice, and strengthen overall network security. 
