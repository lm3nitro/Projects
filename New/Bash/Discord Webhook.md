# Discord Webhook

![Pasted image 20241113150326](https://github.com/user-attachments/assets/e88053d0-39a4-4e90-8b1d-cf92f111307d)

Webhooks allow messages to be posted to a specific channel automatically. When a webhook is created, a dedicated URL is provided that third-party applications can send messages to in order to post a message.

### Scope: 

I will set up a Discord server to create a webhook, and will then deploy an Apache web server. The script I create will monitor the web server's status, incorporating the Discord webhook to send notifications to my desktop and alert a specified Discord channel. In the event of any issues with the web server, the script will automatically trigger a notification, allowing for quick troubleshooting. 

### Tools and Technology:
Discord Server, Apache2, Linux, and Bash

## Discord

First, I went to Discord and picked the server where I will create my webhook: 

![Pasted image 20241113150825](https://github.com/user-attachments/assets/069b0973-a214-43d1-98f1-e149b68fa571)

To create the webhook, these are the steps I used: 
+ Clicked the arrow next to my server name in the upper left corner and chose ***Server Settings*** from the drop down menu.
+ In the side bar, I chose ***Integrations***, then clicked on ***Create Webhook***.
+ The Integrations > Webhooks window is where webhook can be created.
+ I then picked the channel I wanted to post to from the drop down menu.
+ Lastly, I clicked on the ***Copy Webhook URL*** to copy the webhook URL.

## Bash Script:

![Pasted image 20241113151945](https://github.com/user-attachments/assets/26a5668c-75d0-4f23-a9fd-0bd85a62467e)

This script is designed to monitor the status of an Apache web server and perform the following actions:

1. **Check Apache Status:** Regularly checks if the Apache server is running by querying its status.
2. **Discord Webhook Notification:** If the Apache server goes down, the script sends an alert via a Discord webhook to notify a specified channel.
3. **Desktop Notification:** The script also generates a desktop notification to alert the system administrator about the server issue.
4. **Log File Creation:** Logs each check to a local log file, recording the server’s status and timestamp for ongoing monitoring.

After copying the webhook URL, I stored the web-hook URL in a text file to secure it. The webhook URL is essentially a secret endpoint that allows external services to send data to your system. If exposed, malicious actors could exploit it to send unauthorized requests, trigger unwanted actions, or spam your server with false data, potentially compromising the integrity of the system. By keeping the webhook URL "hidden", it ensures that only trusted services and users can interact with the webhook.

![Pasted image 20241113153246](https://github.com/user-attachments/assets/ff5b466c-0715-4512-8a3d-54300e1abfeb)

Running the script in VScode:

![Pasted image 20241113152054](https://github.com/user-attachments/assets/6c272588-0f11-40b7-b360-3f6f2d3801fb)

Below is the script I created:

```bash
#!/usr/bin/env bash


# Discord webhook URL from file

webhook=$(cat discord-webhook-url.txt)


# Local website

local_website="http://lm3nitro.website.local"


# Log file for status checks

log_file="server.log"


# Function to send notifications to Discord

send_discord_notification() {

local website=$1

local status_code=$2

local message="The website ${website} is down. Status Code: ${status_code}"

curl -H "Content-Type: application/json" \

-X POST \

-d '{"content":"'"${message}"'"}' \

"${webhook}"

}

# Function to check website status

web_check() {

while true; do

for website in "${local_website}"; do

# Get the HTTP status code of the website

status_code=$(curl --write-out "%{http_code}" --silent --output /dev/null -L "${website}")


# Check if the website is down

if [[ ${status_code} -ne 200 ]]; then

# Log and notify if the website is down

echo "$(date) - ${website} is down! Status Code: ${status_code}" >> "${log_file}"

notify-send "${website} is down!"

send_discord_notification "${website}" "${status_code}"

else

# Log and notify if the website is up

echo "$(date) - ${website} is up and running!" >> "${log_file}"

notify-send "${website} is up!"

exit 0

fi

done

# Check every 5 seconds

sleep 5

done

}
  
# Start website monitoring

web_check
```

View of the code:

![Pasted image 20241113142610](https://github.com/user-attachments/assets/59974cb0-a187-4cb7-a127-1f47b6b58bbe)

## Apache HTTP Server:

Now that I have both the Webhook URL and the script created, I now needed to set up the Apache Server:

![Pasted image 20241113151806](https://github.com/user-attachments/assets/ce7185df-7f7a-4c06-9bef-837feb581261)

This is the command I used to install it:

``` 
sudo apt install apache2
```

![Pasted image 20241113152004](https://github.com/user-attachments/assets/5685f5a8-f43d-4757-913c-b25a3ce8d8f3)

## Notifications 

I first needed to isntall notify-send. notify-send is a program to send desktop notifications.

![Pasted image 20241113152601](https://github.com/user-attachments/assets/9a2d0c7a-d979-4527-9545-c1299e4024cd)

Once notify-send was isntalled, I executed the script. Upon execution, I could see the notification:

![Pasted image 20241113142330](https://github.com/user-attachments/assets/ac1485bc-d297-4c17-8448-cdc1e09fd939)

I then puposefully took down the server, below is the notification received:

![Pasted image 20241113142524](https://github.com/user-attachments/assets/6c796019-6723-4256-85f1-894b4a51254b)

Taking a look at the server logs, I can also see information on the server status:

![Pasted image 20241113142929](https://github.com/user-attachments/assets/a7149271-550f-4c03-bca5-efec1ba70ef5)

I then verified the notifications being received in Discord:

![Pasted image 20241113150945](https://github.com/user-attachments/assets/6c7b43c6-2eac-420b-b07b-8e949f1f1ed4)

I then restored the server and checked it was running: 

![Pasted image 20241113145753](https://github.com/user-attachments/assets/34f21b1e-5b26-4d22-be20-ab069cf6a3cd)

Once the server was started, the script stopped sending messages to the discord channel.

### Summary: 

This setup is useful because it automates the monitoring of a web server and provides real-time notifications of its status, ensuring quick detection of issues and minimizing downtime. By integrating a Discord webhook, the script sends alerts directly to a specified channel, offering a convenient and efficient way to track server performance from anywhere. 
