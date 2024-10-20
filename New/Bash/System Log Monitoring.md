
```bash

#!/bin/bash

#Creating a variable to hold auth.log file

LOG_FILE="/home/lm3nitro/my_bash_scripts/auth.log"

# Creating an array that searches for two keyword streams in auth.log

KEYWORDS=("Failed password" "authentication failure")

echo "Monitoring log file for suspicious activity..."

cat "$LOG_FILE" | while read LINE; do
    for KEYWORD in "${KEYWORDS[@]}"; do
        if [[ "$LINE" == *"$KEYWORD"* ]]; then
            echo "$(date): Suspicious activity detected"
            echo "$(date): Suspicious activity detected: $LINE" >> /home/lm3nitro/my_bash_scripts/suspicious_activity.log
        fi
    done
done
```

![[Pasted image 20241019195410.png]]
