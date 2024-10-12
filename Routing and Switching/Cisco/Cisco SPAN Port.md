# Cisco SPAN Port 

A SPAN port (Switched Port Analyzer) on a switch is a special port that is used to monitor network traffic. It allows you to copy traffic from one or more source ports or VLANs and send that copied traffic to a designated port (the SPAN port) for analysis.

### Scope:

In my home I currently have a Cisco Switch 3750G. The following will go over the steps I followed in order to configure a SPAN Port on it. 

### Tools and Technology:

Cisco Switch 3750G

![Pasted image 20240416133118](https://github.com/lm3nitro/Projects/assets/55665256/6b784043-d9fd-48bd-baca-455f8766f7f1)

Here is an example topology of the set up I will be doing. I will have 1 PC connected to a port, and I will have the other port set up as a SPAN port with a monitoring device connected to that port.

![Pasted image 20240416133855](https://github.com/lm3nitro/Projects/assets/55665256/b1d33333-adb1-49bc-b93e-9e25c1dfafdb)

## Configuration: 

For informational purposes, this is the current version that I am running:

![Pasted image 20240416134839](https://github.com/lm3nitro/Projects/assets/55665256/5eed5836-b5c4-4f80-95d7-1da335b3e2f9)

First thing I did was take a look at the interfaces:

```
show interfaces
```

![Pasted image 20240416133825](https://github.com/lm3nitro/Projects/assets/55665256/aad9e3f5-6109-41af-9ab5-2e5c82a5d51c)

These are the commands that I will use to configure the SPAN session:

1. This command configures source traffic for the SPAN session. In this case, it selects GigabitEthernet interface 2/0/1 (Gi2/0/1) as the source interface. This means that all traffic passing through interface Gi2/0/1 will be copied (mirrored) to the SPAN session.

```
monitor session 1 source interface Gi2/0/1
```

2. This command configures the destination for the SPAN session. It specifies that the mirrored traffic from the source interface (Gi2/0/1) will be sent to GigabitEthernet interface 2/0/3 (Gi2/0/3). This destination port is where I will connect my monitoring device to capture and analyze the mirrored traffic.

```
monitor session 1 destination interface Gi2/0/3
```

![Pasted image 20240416134151](https://github.com/lm3nitro/Projects/assets/55665256/3b007c12-044d-48db-adca-6a234f8181fc)

3. You can verify the monitor session with the following command:

```
show monitor session 1
```

### Summary:

This was done to show the process and configuration of a SPAN port on a Cisco switch. Configuring a SPAN port on a switch is crucial for effective network monitoring and troubleshooting. Here are just a few reasons and benefits for setting up a SPAN port in detail:

1. Network Traffic Monitoring: SPAN ports allow for real-time monitoring of network traffic, enabling administrators to analyze data as it flows through the network.
  
2. Security Monitoring: SPAN ports can be connected to intrusion detection systems (IDS) or intrusion prevention systems (IPS) to monitor for suspicious activity or unauthorized access attempts.

3. Troubleshooting Network Issues: SPAN ports enable the use of packet capture tools to analyze specific issues like dropped packets, connectivity problems, or application performance.

In summary, configuring a SPAN port on a switch enhances visibility into network operations, improves security posture, aids in troubleshooting, and supports compliance efforts.


