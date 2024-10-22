## Introduction

I created the following script that will monitor the auth.log and search for key streams within it that would be a cause of concern (authentication failure and failed password). Its important to monitor authentication logs because frequent authentication failures could indicate someone is trying to gain unauthorized access to the system, which could lead to a security breach.

## Use Cases

+ Monitor user behavior and adjust security policies accordingly
+ In the event of a security incident, having a detailed record of authentication failures can assist in understanding the nature and scope of the attack.
+ Maintaining logs of authentication failures provides an audit trail that can be reviewed during security assessments

## Script

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

## Testing

After creating the script, I purposefully created failed login attempts to see if the script would pick up on the authentication failures, and I can see the results below: 

![Pasted image 20241019195410](https://github.com/user-attachments/assets/56757c3d-d161-42ea-b4c7-f254a85ca986)

## Summary

Analyzing authentication failures can reveal patterns in user behavior, for example identying legitimate forgotten passwords by users versus brute force attacks. It is especially important to monitor logs for critical assets as an extra layer of security allowing you to be proactive rather than reactive. 
