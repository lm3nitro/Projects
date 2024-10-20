## Introduction

For this exercise, I have an Apache server that is up and running. I created a bash script that will be used to send emails based on the status of my Apache server. For this exercise, I will be confirguing it to send an email to my gmail account.  The email will be triggered whenever the Apache service is stopped. The primary purpose of this script is to promptly inform about critical service changes, enabling a quick response to potential issues or downtime. These are the steps and script I created to perform this task.

## Use Cases:

+ Alert users about planned maintenance windows where services will be temporarily unavailable.
+ Inform users when services are being restarted after updates or configuration changes.
+ Send periodic updates about the health status of critical services, especially if any issues are detected.

## Google Configuration

Prior to starting, I already had a gmail account set up. The only thing that we will need to configure is an App Password. However, in order for the option to be enabled, the gmail account will first need to have 2-step authentication configured. To get this set up sign-in to gmail, go to account settings > security. Ensure that 2-step verification is enabled:

![Pasted image 20241016001208](https://github.com/user-attachments/assets/2513d4ce-53ac-4ada-ab8d-ed256354b16b)

Once that is enabled, you can navigate to app passwords and create a new one. 

> [!NOTE]  
> You can name it anything you want, the name is not relevent. 

![Pasted image 20241016001640](https://github.com/user-attachments/assets/4f417094-7abb-485b-b3c3-e04cdb129de4)

Once the name is added, click `Create`. You will be presented with a password, make sure to keep this password as it will be needed later in the configuration file. 

> [!IMPORTANT]  
> Once the password is presented, there is no way to go back and view it again. If you do not remeber or forgot to make note of the password, you will need to delete the app password and created a new one. 

![Pasted image 20241016001851](https://github.com/user-attachments/assets/0e040009-8438-4e74-a89a-eb8d321f5f32)


## Script

Now that gmail is configured, I created the script that I will be using:

```
#!/usr/bin/env bash

# Check the status of the Apache2 service and save the output
systemctl status apache2 > status_output.txt

# Check if the Apache2 service is running
if grep -q "running" status_output.txt; then    
    echo "The Apache2 service is running."
else
    # Attempt to restart the Apache2 service
    systemctl restart apache2
    
    # Notify that the service was stopped or inactive
    echo "The Apache2 service was stopped, errored, or inactive. It has now been started." | mail -s "Apache2 Service Alert" example.lm3nitro.service@gmail.com
fi

# Clean up the status output file
rm -f status_output.txt
```

Here is a view of the script in Pycharm:
![Pasted image 20241015231342](https://github.com/user-attachments/assets/3147de8c-5d1d-4443-bfd7-42cf9f42886e)

## Installing SSMTP:

Next, I installed SSMTP. SSMTPis a lightweight mail transfer agent designed to send emails from a local machine to a remote SMTP server. 

![Pasted image 20241015234243](https://github.com/user-attachments/assets/76a7f1b2-d5bc-4537-ba08-b7c7f2e9fa0c)

I used the following command to install ssmtp:

```
sudo apt install ssmtp
```

![Pasted image 20241015234452](https://github.com/user-attachments/assets/0ff4f6ba-8174-46b1-b4e7-faa5b1c09fb1)

Once installed, I needed to make changes to the configuration file. Below is a screenshot of the configuration file:

```
  GNU nano 6.2                                                 /etc/ssmtp/ssmtp.conf *                                                        
#
# Config file for sSMTP sendmail
#
# The person who gets all mail for userids < 1000
# Make this empty to disable rewriting.
root=my_example@gmail.com
# The place where the mail goes. The actual machine name is required no 
# MX records are consulted. Commonly mailhosts are named mail.domain.com
mailhub=smtp.gmail.com:587
AuthUser=my_example@gmail.com
Authpass=<google_token_passowrd_here>
UseTLS=YES
UseSTARTTLS=YES

# Where will the mail seem to come from?
rewriteDomain=gmail.com

# The full hostname
hostname=lm3nitro

# Are users allowed to set their own From: address?
# YES - Allow the user to specify their own From: address
# NO - Use the system generated From: address
FromLineOverride=YES
```

The screenshot below reflects the changes made:

![Pasted image 20241015233024](https://github.com/user-attachments/assets/5c29472c-6cbe-401f-ae0e-652c66587521)

I also created a chron job for the script:

![Pasted image 20241015235014](https://github.com/user-attachments/assets/196089c5-6f4d-4d9a-991c-e02e055539b7)

## Testing

To test, I ran the script displayed above. I was able to see that it was running. Since the service was up and running, it did nothing. 

I then stropped the service and ran the script again. This time I can see that the script ran the command to enable the service again and also triggered for an email to be sent:

![Pasted image 20241015235937](https://github.com/user-attachments/assets/c386e8ec-f7fd-47a7-a0af-d77a91bcf6d3)

While performing this test, I also captured the traffic to verify the email being sent:

![Pasted image 20241016000432](https://github.com/user-attachments/assets/382cf83e-0858-4a36-b3ff-bd422ffc4a18)

## Gmail Mailbox:

Here I can see that I was able to receieve an email about the service being down:

![Pasted image 20241015233937](https://github.com/user-attachments/assets/2207acb8-adb2-4f61-8ed0-d7e548f2f4fb)

Here is a look inside what was sent in the email:

![Pasted image 20241015232223](https://github.com/user-attachments/assets/1af1c4bc-5a33-47b2-98e9-fa17fe75cfd1)

## Summary:

In this exercise, I was able to create a script that successfully executes whenever the the Apache service is down. By automatically notifying of service outages, you can quickly react to  potential impacts, gather resources for troubleshooting, and start remediation efforts as soon as the alert is received, enhancing overall service reliability.

This practice helps ensure that services remain accessible and reliable, an important factor in the CIA triad. 



