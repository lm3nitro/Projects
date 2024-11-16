# Macchanger

![Pasted image 20241113181350](https://github.com/user-attachments/assets/50631d96-a165-4b16-a576-347bed10ec50)

Macchanger is a command-line tool used for changing the MAC address of a network interface on Linux-based operating systems. It is a versatile utility that allows users to modify the MAC address of their network interface either randomly or by specifying a custom address, providing an added layer of anonymity or security in networks. Below are the common use cases:

+ Privacy and Anonymity: Changing the MAC address can enhance privacy by preventing tracking based on a device’s unique identifier.
+ Security Testing: During penetration testing or security audits to simulate different devices on a network. This can help simulate different network scenarios and identify potential vulnerabilities.

### Scope
I will be installing macchanger on my Linux VM and creating a script to automate the process of changing the MAC address. The script will be configured to change the MAC address every 10 seconds. Additionally, I will use Wireshark to monitor network traffic and analyze the behavior in real time. This will allow me to observe how frequent MAC address changes impact the network, including any effects on connectivity and traffic patterns.

### Tools and Tehcnology
Macchanger, Linux, Bash, and Wireshark

## Getting Started:

First, I started by installing macchanger:

```
apt install macchanger
```

![Pasted image 20241109164144](https://github.com/user-attachments/assets/ae579f86-65c6-4775-879c-3111613424d7)

Below are the options that can be used with macchanger:

![Pasted image 20241109164317](https://github.com/user-attachments/assets/e99ebe2a-af46-480b-99fc-223036232ecc)

I then manually tested macchanger to see the functionality:

```
macchanger -r ens32
```

![Pasted image 20241109164348](https://github.com/user-attachments/assets/69d73d99-9139-41b5-9597-04845d329d41)

I can see the current and new MAC address. 

## Bash Script:

![Pasted image 20241113181806](https://github.com/user-attachments/assets/83d52939-226e-410b-aed9-191af5c6cc59)

Below is the script I created. In this script, macchanger will change the MAC address every 10 seconds and will default to the ens32 interface. I will also be logging the event with a timestamp for each change.

```bash
#!/usr/bin/env bash

# Check if the script is run as root
if [[ ${UID} -ne 0 ]]; then
    echo "Must be run as root"
    exit 1
fi

# Function to change MAC address
mac_changer() {
    local interface="${1:-ens32}"  # Default to 'ens32' if no interface is provided

    # Log the starting time
    echo "$(date +'%Y-%m-%d %H:%M:%S') - Starting MAC address changer on interface: ${interface}" >> mac.log

    while true; do
        # Change MAC address and log the event with a timestamp
        echo "$(date +'%Y-%m-%d %H:%M:%S') - Changing MAC address for interface: ${interface}" >> mac.log
        macchanger -r ${interface} >> mac.log 2>&1  # Redirect stderr to log as well

        # Sleep for 10 seconds before changing again
        sleep 10
    done
}

# Run the function
mac_changer "$1"  # Pass the interface as an argument, if provided

```

Here is a view of the script in VScode:

![Pasted image 20241113181136](https://github.com/user-attachments/assets/34f4955b-d759-4e49-9c46-c612846c7f05)

I then executed the script. Breakdown of the screenshot below:

+ On the top left side is a screenshot requesting the password. In the script I made it so that it can only be execited by root.
+ On the bottom left I can see the MAC address changes as it occurs.
+ On the top right I am monitoring the interface.
+ On the bottom right I am running tcpdump and capturing the ARP traffic on interface ens32.

![Pasted image 20241109172907](https://github.com/user-attachments/assets/2fa4c847-f4b8-440d-afa4-cc4a754407a1)

Looking at Wireshark, I am monitoring the ARP traffic and checking the changes in real time:

![Pasted image 20241109173716](https://github.com/user-attachments/assets/0d047c16-3c70-4525-82a6-41a97f9f26dd)

### Summary

Using the script, I was able automate the process of using macchanger to change the MAC addresses. The purpose is to streamline the process of periodically changing the MAC address of a network interface, which can enhance privacy, security, and network testing. By automating this process, I was able to observe the effects of frequent MAC address changes on network behavior, such as connectivity and traffic patterns. 
