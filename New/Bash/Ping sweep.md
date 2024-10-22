## Introduction


You need to periodically check which devices are online to ensure that everything is functioning properly. This script will help automate that task.


```
#!/bin/bash

# Local Area Network:
LOCAL_SUBNET="10.10.100"

# CDIR_NET:10.10.100.0/24
# First: 10.10.100.1
# Last: 10.10.100.254
# Broadcast: 10.10.100.254
# Mask: 255.255.255.0

#Looping trough the LOCAL_SUBNET using {}
for IP in $(seq 1 254); do
    ping -c 1 "$LOCAL_SUBNET.$IP" | grep "bytes from" | cut -d " " -f 4 | cut -d ":" -f 1 & # Runs each ping in the background
done
# Waiting for all ping background jobs to finish before exiting
wait
```

![Pasted image 20241020164654](https://github.com/user-attachments/assets/4c86e4f7-c296-4aa3-ba6c-fa989d52c7af)


![Pasted image 20241020164631](https://github.com/user-attachments/assets/d7a8f321-6437-4a0f-9986-4fe3151b4b31)



This log can then be used for further analysis, inventory tracking, or troubleshooting.
