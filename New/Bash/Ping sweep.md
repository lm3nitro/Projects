## Introduction

I created the following script that sends an ICMP request (ping) to devices in a network to determine which hosts are online and responsive, providing a quick assessment of the network's status. This is useful to ensure that all devices are functioning properly, identify potential issues, and maintaining network reliability.

## Use Cases:

+ For critical infrastructure or services, regular pings can ensure that essential systems remain online and accessible.
+ When changes are made to network configurations or security settings, pinging can quickly verify that devices are still operational.

## Script

Below is the script I created. Here I am scanning all the devices in my network (10.10.100.0/24). I am then taking the output and modifying it to display only the relevent information I am interested seeing by using grep:

```bash
#!/bin/bash

# Local Area Network:
LOCAL_SUBNET="10.10.100"

# CDIR_NET:10.10.100.0/24
# First: 10.10.100.1
# Last: 10.10.100.254
# Broadcast: 10.10.100.255
# Mask: 255.255.255.0

#Looping trough the LOCAL_SUBNET using {}
for IP in $(seq 1 254); do
    ping -c 1 "$LOCAL_SUBNET.$IP" | grep "bytes from" | cut -d " " -f 4 | cut -d ":" -f 1 & # Runs each ping in the background
done
# Waiting for all ping background jobs to finish before exiting
wait
```

Here is another view of the script in VS Code. After executing the script, this is the output that it provides, a clear view of all the IPs that reponded to the ping:

![Pasted image 20241020164654](https://github.com/user-attachments/assets/4c86e4f7-c296-4aa3-ba6c-fa989d52c7af)

While running the script, I was also capturing the traffic in real time. Here is a view of the traffic. This log can then be used for further analysis, inventory tracking, and troubleshooting.

![Pasted image 20241020164631](https://github.com/user-attachments/assets/d7a8f321-6437-4a0f-9986-4fe3151b4b31)

## Summary

Going through this exercise allowed me to see valuable insights into my network’s health and device status. This practice is essential for security as it helps identify unauthorized devices, detect anomalies in network behavior, and ensure that critical systems remain operational.
