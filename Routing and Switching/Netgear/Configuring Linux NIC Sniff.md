
# Linux NIC Sniff

### Linux Interface configuration to capture packets:

Various NIC/driver combinations may have different default settings for these offload features. It's important to verify your interface settings sooner rather than later to avoid a situation where you require a full packet capture but find out later that it's not available. You can check this by using the 'ethtool' command with the '-k' option (lowercase 'k') on the specific interface. For instance, to check 'eth0':
```
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
```

>#### Note: You should repeat this for every interface in your system, as you may have NICs from different manufacturers with different defaults.

You can set these options using the "-K" (upper-case K) option to ethtool and specify which option you'd like to set.  For example, to disable tcp-segmentation-offload for eth0:

```
> ethtool -K eth0 tso off
```

You can set multiple options in one "ethtool" command, but this can be problematic if your card doesn't support all of the settings. To avoid this, you could invoke ethtool for each option like this:
```
> ethtool -K eth0 rx off  
> ethtool -K eth0 tx off  
> ethtool -K eth0 sg off  
> ethtool -K eth0 tso off  
> ethtool -K eth0 ufo off  
> ethtool -K eth0 gso off  
> ethtool -K eth0 gro off  
> ethtool -K eth0 lro off 
```
Or we could simply wrap the ethtool command in a for-loop like this:

```
> for i in rx tx sg tso ufo gso gro lro; do ethtool -K eth0 $i off; done
```

### Enable Promiscuous Mode

1. To enable the promiscuous mode on the physical NIC, run the following command on the Server text console:  

```
ip link set eth0 promisc on
```
OR
```
sudo ifconfig eth0 promisc
```
```
sudo tail -f /var/log/syslog
kernel: [ 2155.176013] device eth0 left promiscuous mode
```
```
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

In summary, configuring a NIC on a Linux system for promiscuous mode and offloading can be crucial for various networking and security purposes. 

*Promiscuous Mode* allows the NIC to capture and inspect all network traffic passing through the interface, which is   particularly useful for network analysis, monitoring, and troubleshooting tasks. 

*Offloading* refers to delegating certain networking tasks, such as checksum calculation or segmentation, to the NIC itself, which can significantly reduce the CPU overhead and improve network throughput.

Configuring these settings appropriately ensures efficient and effective network operations while maximizing system performance and security capabilities.
