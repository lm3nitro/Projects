

SIgn-in to google, go to account settings > security
Ensure that 2-step verification is enabled, as this is required in order to create an app password. 

![Pasted image 20241016001208](https://github.com/user-attachments/assets/2513d4ce-53ac-4ada-ab8d-ed256354b16b)


Once that is enabled, you can navigate to app passwords and create a new one:

![Pasted image 20241016001640](https://github.com/user-attachments/assets/4f417094-7abb-485b-b3c3-e04cdb129de4)


Here you can choose any name you want and then click `Create`. You will be presented with a password, make sure to keep this password as it will be needed later in the configuration file:

![Pasted image 20241016001851](https://github.com/user-attachments/assets/0e040009-8438-4e74-a89a-eb8d321f5f32)




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


![Pasted image 20241015231342](https://github.com/user-attachments/assets/3147de8c-5d1d-4443-bfd7-42cf9f42886e)


### Installing SSMTP:

![Pasted image 20241015234243](https://github.com/user-attachments/assets/76a7f1b2-d5bc-4537-ba08-b7c7f2e9fa0c)


is a simple and lightweight SMTP client designed for sending emails from a command line or a script. It acts as a minimalistic solution for applications that need to send emails without the overhead of a full-fledged mail server.



```
sudo apt install ssmtp
```

![Pasted image 20241015234452](https://github.com/user-attachments/assets/0ff4f6ba-8174-46b1-b4e7-faa5b1c09fb1)


### Configuration file:

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


![Pasted image 20241015233024](https://github.com/user-attachments/assets/5c29472c-6cbe-401f-ae0e-652c66587521)




![Pasted image 20241015235014](https://github.com/user-attachments/assets/196089c5-6f4d-4d9a-991c-e02e055539b7)



# Testing the alerts



![Pasted image 20241015235937](https://github.com/user-attachments/assets/c386e8ec-f7fd-47a7-a0af-d77a91bcf6d3)


### Network Traffic:

![Pasted image 20241016000432](https://github.com/user-attachments/assets/382cf83e-0858-4a36-b3ff-bd422ffc4a18)


### Gmail Mailbox:


![Pasted image 20241015233937](https://github.com/user-attachments/assets/2207acb8-adb2-4f61-8ed0-d7e548f2f4fb)


![Pasted image 20241015232223](https://github.com/user-attachments/assets/1af1c4bc-5a33-47b2-98e9-fa17fe75cfd1)
