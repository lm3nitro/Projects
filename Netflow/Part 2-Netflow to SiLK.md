# Cisco Netflow to SiLK (rwflowpack)



![Pasted image 20240529222403](https://github.com/lm3nitro/Projects/assets/55665256/169c6648-bc17-4040-8489-ae539a8e8e18)




NetFlow configuration:


![Pasted image 20240527231718](https://github.com/lm3nitro/Projects/assets/55665256/4dcb6f85-82b6-43a8-a251-5fdc167f29c0)



Interfaces:

![Pasted image 20240527231745](https://github.com/lm3nitro/Projects/assets/55665256/05fc5d89-5dc2-40fd-a42b-a746af965bda)





ip flow-cache timeout active 1
ip flow-cache timeout inactive 15
ip flow-capture vlan-id
ip flow-capture mac-addresses
ip flow-export version 9 origin-as
ip flow-export destination 172.16.10.10 9996   # rwflowpack IP and port


Run the following command to confirm the configuration:


show ip cache flow
show ip flow interface
show ip flow export



show ip flow export template

![Pasted image 20240527232533](https://github.com/lm3nitro/Projects/assets/55665256/244e58c6-f0e5-4cc6-9f90-c0a842374be6)



show flow interface - Displays information about the NetFlow interfaces



![Pasted image 20240527224019](https://github.com/lm3nitro/Projects/assets/55665256/4ae5be53-59ae-4de7-bea8-73043831c549)



show flow exporter - Displays information about the exporters and the statistics


![Pasted image 20240527224056](https://github.com/lm3nitro/Projects/assets/55665256/953ec4c5-ccbc-4a34-a1ac-b6a9a5d52671)




Show details summary of the NetFlow accounting statistics:

show ip cache verbose flow

![Pasted image 20240527224232](https://github.com/lm3nitro/Projects/assets/55665256/a4a46348-f88d-43ac-823c-8c7d255d3e84)




After the compilation of Silk covered on projects above. This is the only changes to be able revice traffic from Router NETFLOW V9 COMPATIBLE.



### Once Silk has been installed update Dynamic Linker.

Finally, you should update the cache of the dynamic linker. If you do not, it may be necessary to set the LD_LIBRARY_PATH environment variable to /usr/local/lib when you use SiLK or YAF.

Typically an entry for /usr/local/lib will already exist in the /etc/ld.so.conf.d/ directory. To confirm:

$ grep local /etc/ld.so.conf.d/*
/etc/ld.so.conf.d/libc.conf:/usr/local/lib

![Pasted image 20240527223036](https://github.com/lm3nitro/Projects/assets/55665256/b2f53a13-f4a0-4cce-b898-9077bee7c899)

To update the cache in this case, run the ldconfig program:



# Configure SiLK:

The first step to configuring SiLK is to create the data repository directory (/var/silk/data) and add the silk.conf file, which defines how your data is stored (see the manual page for details). Use the default silk.conf file for the twoway site, which is installed at /usr/local/share/silk/twoway-silk.conf. You may edit the sensor descriptions if desired. The default settings cause the SiLK analysis program rwfilter to consider only incoming data unless the user provides the --type or --flowtypes switch. If you want rwfilter to look at both incoming and outgoing data by default, modify the default-types line to include in inweb out outweb. If desired, also add int2int ext2ext.

mkdir -p /var/silk/data
chmod go+rx /var/silk /var/silk/data
cp /usr/local/share/silk/twoway-silk.conf /var/silk/data/silk.conf

# Configure rwflowpack:


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



![Pasted image 20240527215203](https://github.com/lm3nitro/Projects/assets/55665256/5ba4232b-b156-4a38-a1dd-1e388f053557)


Move this file into place.

SiLK comes with traditional "init script" start-up files, with two files per daemon: One file is invoked by the system to start the daemon, and the other contains configuration settings used by the first. Copy rwflowpack's start-up script to /etc/init.d/rwflowpack, and copy the configuration settings script to /usr/local/etc/rwflowpack.conf:
mv sensors.conf /var/silk/sensors.conf

cd /usr/local
cp share/silk/etc/init.d/rwflowpack /etc/init.d/rwflowpack
cp share/silk/etc/rwflowpack.conf /usr/local/etc/rwflowpack.conf

Open the /usr/local/etc/rwflowpack.conf file in an editor and change these variables to the values shown here:

ENABLED=1
statedirectory=/var/silk
SENSOR_CONFIG=/var/silk/sensors.conf
ARCHIVE_DIR=  # empty
LOG_TYPE=legacy
LOG_DIR=/var/log
PID_DIR=/var/run


Note:

Start rwflowpack (a message about "contains no runlevels, aborting" is non-fatal):

systemctl enable rwflowpack
systemctl start rwflowpack.service


systemctl status rwflowpack

![Pasted image 20240527222603](https://github.com/lm3nitro/Projects/assets/55665256/1cafd283-bd5f-48ae-b90e-6255fe45f752)


Verify port listening for rwflowpack:

![Pasted image 20240527223225](https://github.com/lm3nitro/Projects/assets/55665256/334ca804-f222-454a-aef5-e862d9c49f0e)

Verify  if the NetFlow-v9 is coming from the Router:

![Pasted image 20240527223533](https://github.com/lm3nitro/Projects/assets/55665256/f91eee7f-481b-48ba-a234-770c847c18dc)




Run the following query to get data for the current day:


/usr/local/bin/rwfilter --sensor=S0 --type=all --all=stdout \
| rwcut --tail-recs=10


![Pasted image 20240527222712](https://github.com/lm3nitro/Projects/assets/55665256/e0945dce-dbf9-4e30-8d3f-d9c7bcaf25af)




# Data directory:

![Pasted image 20240527221824](https://github.com/lm3nitro/Projects/assets/55665256/e4ef52fe-a4c6-475c-b18d-1d7910115d5a)



The silk.conf file and packlogic-twoway.so plug-in define a single class, all.

The type assigned to a flow record within the all class depends on the how the record moves between the networks, and the types follow from the table above:

in, inicmp, inweb
Incoming traffic. The traffic is split into multiple types, and these types allow the analysts to query a subset of the flow records depending on their needs. Each incoming flow record is split into the one of incoming types using the following rules:

inweb
Contains traffic where the protocol is TCP (6) and either the source port or the destination port is one of 80, 443, or 8080

inicmp
Contains flow records where either the protocol is ICMP (1) or the flow record is IPv6 and the protocol is ICMPV6 (58). By default, the inicmp and outicmp types are not used by the packlogic-twoway.so plug-in.

in
Contains all other incoming traffic.

out, outicmp, outweb
Outgoing traffic. The traffic is split among the types using rules similar to those for incoming traffic.

innull
Blocked incoming traffic

outnull
Blocked outgoing traffic

ext2ext
Strictly external traffic

int2int
Strictly internal traffic

other
Either traffic from the null network or traffic to or from the unknown network



# Queries 

Query #1 

filtering for dest port 443 in outweb dir
rwfilter --dport=443 ow-S0_20240528.00 --pass=stdout | rwcut --field=1-9 --num-recs=10



![Pasted image 20240527224953](https://github.com/lm3nitro/Projects/assets/55665256/89e29bca-3d7c-40e9-a3a0-5db2f279989e)

Query #2

Filtering for sport range

rwfilter --sport=49993-50000 ow-S0_20240528.00 --pass=stdout | rwcut --field=1-9 --num-recs=10


![Pasted image 20240527225305](https://github.com/lm3nitro/Projects/assets/55665256/f3f4905f-a3fd-4823-b620-fca37b6c20ca)


Query #3

Print summary of traffic stats

rwfilter --print-volume-stat ow-S0_20240528.00 --proto=0-255


![Pasted image 20240527225604](https://github.com/lm3nitro/Projects/assets/55665256/c7e10da3-24e7-4fd3-898e-4f749f1b4d4d)


Query #4

View All file info

rwfileinfo ow-S0_20240528.00

![Pasted image 20240527225804](https://github.com/lm3nitro/Projects/assets/55665256/275a2c63-2478-48ba-afd5-1c96e365cb31)


Query #5


rwcount interpolating how much traffic is in each bin. Splits each flow proportionally

 rwfilter ow-S0_20240528.00 --all=stdout | rwcount --bin-size=500


![Pasted image 20240527230430](https://github.com/lm3nitro/Projects/assets/55665256/55e31c86-169e-4b7f-a5f4-c1be4743ce84)


Query #6

counting number of flows for a particular key. The key is the --field option

rwfilter ow-S0_20240528.00 --all=stdout | rwuniq --field=sip,dip,proto | head -10

![Pasted image 20240527230923](https://github.com/lm3nitro/Projects/assets/55665256/88611146-9eac-4001-9bc5-edab3db67b70)
