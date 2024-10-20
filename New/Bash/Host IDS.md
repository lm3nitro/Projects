
```bash 
#!/bin/bash

WATCHED_FILES=("/home/lm3nitro/my_bash_scripts/etc/passwd" "/home/lm3nitro/my_bash_scripts/etc/shadow")

CHECKSUM_FILE="/home/lm3nitro/my_bash_scripts/my_file_checksums.txt"

# Generating checksums files if not exists

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
![Pasted image 20241019202837](https://github.com/user-attachments/assets/4544ae45-72f2-4fd8-b103-19638742833b)

![Pasted image 20241019202649](https://github.com/user-attachments/assets/68119777-6ee9-4f3d-b7be-44157c12907f)
