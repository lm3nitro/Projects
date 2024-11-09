# Tcpdump Automation

![Pasted image 20241014163054](https://github.com/user-attachments/assets/6e5b4b02-a994-4248-bce9-0631f5409575)

Tcpdump is a powerful command-line tool used for network packet analysis. It allows users to capture and inspect the data traffic that flows through a network interface on a system. It’s also lightweight and flexible, making it ideal for both simple diagnostics and advanced analysis.

### Scope:
In this exercise, I will set-up a continuous network traffic capture aimed at continously monitoring the network. The focus is to ensure that only recent network activity is always available for analysis, while older, less relevant data is automatically discarded. I will cover a couple of ways in which tcpdump can be used to limit the amount and size of files that are generated when continuously monitoring. I will also use a script that will monitor the directory where the files are stored and ensure that stay under a specified amount. 

### Tools and Technology:
Tcpdump, Linux, and Bash

## Limiting File Size and Amount

Limiting the file size and number of files when using tcpdump can be crucial for several practical reasons, especially in situations where you need to continuously capture network traffic over long periods of time. 

Reasons to implement:
+ Prevent Disk Space Exhaustion
+ Manageability of Capture Files
+ Easier File Transfer and Analysis
+ Efficient Analysis
+ Logging and Long-Term Monitoring

In order to implement a set number of files and file size, I started by creating a service file:

```
sudo nano /etc/systemd/system/tcpdump.service
```

![Pasted image 20241014135033](https://github.com/user-attachments/assets/6806685f-6976-4526-8359-3efde79a3b88)

In the service file, I entered the following:

```                                                                   
[Unit]
Description=TCPDump Packet Capture Service

[Service]
ExecStart=/usr/bin/tcpdump -i ens32  -W 10 -C 1 -w /home/elk/pcap/trace.pcap -Z elk
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

This will create 10 files at all times of 1 mb each and keep rotating them. 

In order for the configuration to be applied, I reloaded the systemd manager configuration:

```
sudo systemctl daemon-reload
```

Enabled the service:

```
sudo systemctl enable tcpdump.service
```

![Pasted image 20241014123942](https://github.com/user-attachments/assets/8ae1e58b-563f-46d9-94cf-3e86cb489470)

I then verified the status:

```
sudo systemctl status tcpdump.service
```

![Pasted image 20241014124530](https://github.com/user-attachments/assets/154d7273-fb64-4dad-9e93-24753c86b2f9)

I then stopped the service:

```
sudo systemctl stop tcpdump.service
```

![Pasted image 20241014124209](https://github.com/user-attachments/assets/506ad85d-2188-476f-ae89-ca3f038de212)

Restarted:

```
sudo systemctl restart  tcpdump.service
```

Below are the trace files:

![Pasted image 20241014135123](https://github.com/user-attachments/assets/2b2ca796-3302-4276-b082-abb8ff638b23)

## Capturing Based on Time:

Here, I will be creating a new trace file every 10 seconds that will invlude the timestamp. The timestamp-based filenames are especially useful for chronologically ordered logs, as they tell you exactly when the capture started, and which packets are in each file. By rotating the capture files every 10 seconds (or any other time interval), you can limit the file size and keep them small and manageable, preventing overly large files and making it easier to analyze specific time slices of traffic.

```
tcpdump -ni ens32 -w tracepcap-%Y-%m-%d-%H-%M-%S -G 10 -Z elk &  
```

![Pasted image 20241014152900](https://github.com/user-attachments/assets/a0ad3886-a0ef-41cf-b0ae-aa142e5009cd)

The challenge with capturing traffic based on time is that tcpdump lacks a built-in mechanism to limit the amount of the capture files. The only way to control the number of capture files is by manually deleting older files once the desired file count is reached.

![Pasted image 20241014163029](https://github.com/user-attachments/assets/c9d848c2-ecd5-473a-9a76-7c18d7ab78ae)

To limit the number of capture files, I created a Bash script that monitors the directory where the files are stored. The script automatically deletes older files to ensure that only the specified number of files are retained at all times:

```bash
#!/bin/bash

# Directory containing all trace files:
DIRECTORY="/home/elk/pcap"
MAX_FILES=10  # Seting the maximum number of files to keep

# Counting the number of files in the directory:
FILE_COUNT=$(find "$DIRECTORY" -type f -name "trace*" | wc -l)

# Checking if the file count exceeds the maximum number of files to keep
if [ "$FILE_COUNT" -gt "$MAX_FILES" ]; then
    # Calculating how many files to delete
    FILES_TO_DELETE=$((FILE_COUNT - MAX_FILES))

    # Deleting the oldest files
    find "$DIRECTORY" -type f -name "trace*" -printf '%T+ %p\n' | sort | head -n "$FILES_TO_DELETE" | cut -d' ' -f2- | xargs rm -f
fi
```

![Pasted image 20241014153932](https://github.com/user-attachments/assets/239a2fe6-a679-4edb-967b-9677f0f129ee)

I then created a cronjob that wil execute the script every 1 minute.

```
crontab -e
```

![Pasted image 20241014151013](https://github.com/user-attachments/assets/e74189ad-7809-4e9b-be27-260d4f13f2b1)

I then tested the script:

1. Reading all pcap files with a for loop:

```
for pcap in /home/elk/pcap/tracepcap*; do tcpdump -nr "$pcap" icmp  ; done 
```

![Pasted image 20241014161606](https://github.com/user-attachments/assets/488c11b5-f21b-4c60-9876-67156d084894)

As seen above, I am sending ICMP to two destinations (1.1.1.1 and 9.9.9.9). I am also reading all the trace files on the right side with a for loop. On the top right corner is the screenshot showing where the files are getting generated, however, the script is also running and removing older files as new ones are created in order to ensure that only the specified number of files are retained at all times. 

### Summary:

In this exercise, I was able to modify the tcpdump service in order to limit the amount of capture files created based on time, date, and number of files. Doing this has several key benefits, especially when dealing with long-duration or high-traffic network monitoring. Limiting file sizes ensures that individual capture files remain manageable, preventing them from growing too large and becoming difficult to handle, transfer, or analyze. Rotating files based on time ensures that data is captured in smaller, more manageable chunks, which simplifies analysis and reduces the risk of losing large amounts of data in the event of a failure.

I also created a script that will continuously run and monitor the file directory to ensure that the number of files are kept under a specified amount. This approach prevents manual intervention and avoids the risk of running out of storage space, which could disrupt the monitoring process or cause the system to fail. 

