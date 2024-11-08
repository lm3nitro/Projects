
```
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


![Pasted image 20241108150041](https://github.com/user-attachments/assets/dd1e4543-fe4c-4df9-95d5-d27f607b20e5)

![Pasted image 20241108163808](https://github.com/user-attachments/assets/f4beb839-c093-4aac-b483-5565fba7bf3d)

![Pasted image 20241108162847](https://github.com/user-attachments/assets/ddf428de-6471-4657-b083-4e1839b7f341)

