
# LINUX 

Linux Interface configuration for Capture packets:


Some NIC/driver combinations may disable these offload settings by default, while others enable it by default.  You should check your interface now before you get into a situation where you really need that full packet capture and find out that you don't actually have it.  To check, run ethtool with the "-k" (lower-case k) option on the interface you'd like to check.  For example, to check eth0:

> ethtool -k eth0  
> Offload parameters for eth0:  
> rx-checksumming: on  
> tx-checksumming: on  
> scatter-gather: on  
> tcp-segmentation-offload: on  
> udp-fragmentation-offload: off  
> generic-segmentation-offload: on  
> generic-receive-offload: on  
> large-receive-offload: off

You should repeat this for every interface in your system, as you may have NICs from different manufacturers with different defaults.


You can set these options using the "-K" (upper-case K) option to ethtool and specify which option you'd like to set.  For example, to disable tcp-segmentation-offload for eth0:

> ethtool -K eth0 tso off

You can set multiple options in one "ethtool" command, but this can be problematic if your card doesn't support all of the settings.  To avoid this, you could invoke ethtool for each option like this:

> ethtool -K eth0 rx off  
> ethtool -K eth0 tx off  
> ethtool -K eth0 sg off  
> ethtool -K eth0 tso off  
> ethtool -K eth0 ufo off  
> ethtool -K eth0 gso off  
> ethtool -K eth0 gro off  
> ethtool -K eth0 lro off 

Or we could simply wrap the ethtool command in a for-loop like this:

> for i in rx tx sg tso ufo gso gro lro; do ethtool -K eth0 $i off; done



# Enable Promiscuous Mode

1. To enable the promiscuous mode on the physical NIC, run the following command on the Server text console:  

```
ip link set eth0 promisc on

OR
sudo ifconfig eth0 promisc
```


```
sudo tail -f /var/log/syslog
kernel: [ 2155.176013] device eth0 left promiscuous mode

netstat -i
Kernel Interface table
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       1500 0     29172      0      0 0         29850      0      0      0 BMRU
```

 Run the **ifconfig** command and notice the outcome:  
    eth0   Link encap:Ethernet  HWaddr 00:1D:09:08:94:8A  
              inet6 addr: fe80::21d:9ff:fe08:948a/64 Scope:Link  
              UP BROADCAST RUNNING PROMISC MULTICAST  MTU:1500  Metric:1  
              RX packets:23724 errors:0 dropped:0 overruns:0 frame:0  
              TX packets:7517 errors:0 dropped:0 overruns:0 carrier:0  
              collisions:0 txqueuelen:1000  
              RX bytes:2169478 (2.0 MiB)  TX bytes:5423377 (5.1 MiB)  
              Interrupt:17
    
