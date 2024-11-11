#Arping

Arping is a command-line utility used to send ARP (Address Resolution Protocol) requests to a network host to discover its MAC address or check its presence on the local network. Arping uses ARP requests to query the MAC address corresponding to an IP address.

## Script:

I created the following bash script that will perform an arp scan of a network and then place the results into a log with the time and date information of when the scan was performed. In some cases, unauthorized devices may connect to your network. By periodically scanning and logging, you can check for any unfamiliar devices. Below is the script:

```bash
#!/usr/bin/env bash

# Print script info
echo "Starting the script $0 at $(date '+%d/%m/%Y %H:%M:%S')"

# Create a function to scan the network
scan() {
    # Local variables
    lan_net=()
    ip_prefix="10.10.10.100"
    w_int="wlp2s0"
    temp_file="temp.log"

    # Create empty log file if exists
    if [ -f "$temp_file" ]; then
        rm "$temp_file"
    fi

    # Populate the array with IP addresses
    for ip in {1..254}; do
        lan_net+=("${ip_prefix}.${ip}")
    done

    # Perform ARP scanning in the background for each IP
    for net in "${lan_net[@]}"; do
        arping -i "${w_int}" -c 1 "${net}" >> ${temp_file} &
    done

    # Wait for all background jobs to finish
    wait
}

# Start scanning
scan

# Display the results
echo -e "-----> Results <------\n"
cat ${temp_file} | grep -E "bytes from" | awk '{ print $4 $5 }'

# Final output and log backup
echo "Script finished at: $(date '+%d/%m/%Y %H:%M:%S')"

# Backup the results
cp ${temp_file} backup_$(date '+%d-%m-%Y-%H-%M-%S').log

# Remove the temporary log file
rm ${temp_file}
```

Here is a view of the script in VScode:

![Pasted image 20241108150041](https://github.com/user-attachments/assets/dd1e4543-fe4c-4df9-95d5-d27f607b20e5)

Below is a snip of the arp scan when the script was esecuted:

![Pasted image 20241108163808](https://github.com/user-attachments/assets/f4beb839-c093-4aac-b483-5565fba7bf3d)

I executed the script multiple times and could seer the logs that were getting created with the timestamp information:

![Pasted image 20241108162847](https://github.com/user-attachments/assets/ddf428de-6471-4657-b083-4e1839b7f341)

### Summary:

This script automates the process of running an ARP scan at specified intervals, with the results saved to log files that accumulate over time. This enables trend analysis and facilitates monitoring network growth. By creating a historical record of network device activity, the script supports both proactive network management and effective response to security incidents. The accumulating logs provide valuable insights into device behavior, helping to identify anomalies and track changes in the network over time.
