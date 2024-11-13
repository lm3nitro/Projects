

![Pasted image 20241113181350](https://github.com/user-attachments/assets/50631d96-a165-4b16-a576-347bed10ec50)


## Change a MAC Address:

Use case:

Privacy and Anonymity: Changing the MAC address can enhance our privacy by preventing tracking based on our device’s unique identifier.

Security Testing:  During penetration testing or security audits to simulate different devices on a network. This can help simulate different network scenarios and identify potential vulnerabilities.

The macchanger tool is designed to alter the MAC address of a network interface. It provides a straightforward way to change the unique identifier of a device on a network.

![Pasted image 20241109164144](https://github.com/user-attachments/assets/ae579f86-65c6-4775-879c-3111613424d7)



![Pasted image 20241109164317](https://github.com/user-attachments/assets/e99ebe2a-af46-480b-99fc-223036232ecc)




![Pasted image 20241109164348](https://github.com/user-attachments/assets/69d73d99-9139-41b5-9597-04845d329d41)


### Bash Script:

![Pasted image 20241113181806](https://github.com/user-attachments/assets/83d52939-226e-410b-aed9-191af5c6cc59)


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


## Vscode:


![Pasted image 20241113181136](https://github.com/user-attachments/assets/34f4955b-d759-4e49-9c46-c612846c7f05)


## Running the script:

![Pasted image 20241109172907](https://github.com/user-attachments/assets/2fa4c847-f4b8-440d-afa4-cc4a754407a1)


### ARP traffic:

![Pasted image 20241109173716](https://github.com/user-attachments/assets/0d047c16-3c70-4525-82a6-41a97f9f26dd)

