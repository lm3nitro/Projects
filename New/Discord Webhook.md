
![[Pasted image 20241113150326.png]]

Webhooks **let you post messages to a specific channel automatically**. When you create a webhook, you get a dedicated URL that third-party applications can send messages to in order to post a message.



1. First, open Discord and pick the server where you want to create your webhook.

![[Pasted image 20241113150825.png]]

2. Click the arrow next to your server name in the upper left corner and choose **Server Settings** from the drop down menu.
3. In the side bar, choose **Integrations,** then click on **Create Webhook**.
4. The Integrations > Webhooks window allows creating a new webhook.
5. 1. Pick the channel you want to post to from the drop down menu.
6. lick on the **Copy Webhook URL** button and a webhook URL will be copied

##  Bash Script:

![[Pasted image 20241113151945.png]]



### Information about the script:

**Overview:** This script is designed to monitor the status of an Apache web server and perform the following actions:

1. **Check Apache Status:** Regularly checks if the Apache server is running by querying its status.
2. **Discord Webhook Notification:** If the Apache server goes down, the script sends an alert via a Discord webhook to notify a specified channel.
3. **Desktop Notification:** The script also generates a desktop notification to alert the system administrator about the server issue.
4. **Log File Creation:** Logs each check to a local log file, recording the server’s status and timestamp for ongoing monitoring.


I stored the web-hook URL in a text file to secure it.

![[Pasted image 20241113153246.png]]

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



### Installing Apache HTTP Server:

![[Pasted image 20241113151806.png]]


``` 
sudo apt install apache2
```
![[Pasted image 20241113152004.png]]


### Running the script in VScode:


![[Pasted image 20241113152054.png]]



![[Pasted image 20241113142610.png]]





# Notifications sent to the Desktop: 


### Installing notify-send :

notify-send : is  a program to send desktop notifications



![[Pasted image 20241113152601.png]]


Notification 1:
![[Pasted image 20241113142330.png]]


Notification 2:

![[Pasted image 20241113142524.png]]


### Server logs:


![[Pasted image 20241113142929.png]]




### Discord notifications:




![[Pasted image 20241113150945.png]]
#  Restore Server Status:

#### Once the server is started the script will stop sending messages to the discrod channel:

 ![[Pasted image 20241113145753.png]]
