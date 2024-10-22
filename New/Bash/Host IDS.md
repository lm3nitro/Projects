## Introduction

In the following bash script, I will be monitoring key system files (/etc/passwd, /etc/shadow) by generating a checksum for them and ensuring that no authorized changes are made by comparing the hash. By generating and monitoring hashes, the script provides a reliable method to detect unauthorized changes, corruption, or tampering of important files.

## Use Cases: 

+ This can be used to monitor files containing personal or financial information to detect unauthorized changes or access attempts, enhancing data protection efforts.
+ When transferring files over networks, the script can verify the integrity of the transferred files by comparing hashes, ensuring that files were not corrupted or altered during transit.
+ System administrators can track changes to configuration files (like web server or database configurations) to ensure that only authorized modifications are made, preventing misconfigurations that could lead to outages.

## SCRIPT

Below is the script I created/used. As seen, the md5 when then be generated in order to be able to compare:

```bash
#!/bin/bash

WATCHED_FILES=("/home/lm3nitro/my_bash_scripts/etc/passwd" "/home/lm3nitro/my_bash_scripts/etc/shadow")

CHECKSUM_FILE="/home/lm3nitro/my_bash_scripts/my_file_checksums.txt"

# Generating checksums for the files if not already existing

if [ ! -f "$CHECKSUM_FILE" ]; then
    for FILE in "${WATCHED_FILES[@]}"; do
        md5sum "$FILE" >> "$CHECKSUM_FILE"
    done
    echo "Checksum file created."
    exit 0
fi

# Checking for changes
echo "Checking for unauthorized changes..."
for FILE in "${WATCHED_FILES[@]}"; do
    NEW_CHECKSUM=$(md5sum "$FILE" | awk '{ print $1 }')
    OLD_CHECKSUM=$(grep "$FILE" "$CHECKSUM_FILE" | awk '{ print $1 }')
    if [ "$NEW_CHECKSUM" != "$OLD_CHECKSUM" ]; then
        echo "$(date): Unauthorized change detected in $FILE!" >> /home/lm3nitro/my_bash_scripts/intrusion_detection.log
    fi
done
```

Here is another view of the script in VSCode:

![Pasted image 20241019202837](https://github.com/user-attachments/assets/4544ae45-72f2-4fd8-b103-19638742833b)

As seen in the screenshot above, I tested the script and observed that it successfully created checksums for the specified files. I then intentionally modified the files, as shown in the screenshot below. This modification triggered the script to detect the changes when it ran again.

![Pasted image 20241019202649](https://github.com/user-attachments/assets/68119777-6ee9-4f3d-b7be-44157c12907f)

## Summary:

Through testing the script, I learned the critical importance of continuous file monitoring. This exercise demonstrates how checksums effectively verify file integrity, allowing for quick alerts when modifications occur. Automating this process not only increases efficiency but also reduces the risk of human error, while maintaining an audit trail for compliance purposes. Overall, this type script is essentil for early threat detection, safeguarding sensitive information, and ensuring adherence to standards and best practices. 
