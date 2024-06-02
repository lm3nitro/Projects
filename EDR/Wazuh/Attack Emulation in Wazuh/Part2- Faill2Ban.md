# Fail2Ban

<img width="418" alt="Screenshot 2024-06-02 at 1 34 19 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/4c6f7e45-9357-48e8-8df8-655b426de5cf">

Fail2Ban is an open source intrusion prevention software that monitors server log files for intrusion attempts and other suspicious activities. 
The fail2ban application monitors server log files for intrusion attempts and other suspicious activity. Fail2ban, also helps to secure your server against unauthorized access attempts. It is particularly effective in reducing the risk from scripted attacks and botnets.

Here are some of the key features:
+ Log Monitoring
+ Automatic Banning
+ Configurable Filters and Actions
+ Multi-Serve Protection
+ Email Notification



![Pasted image 20240428180638](https://github.com/lm3nitro/Projects/assets/55665256/4e8c7012-cae1-4479-94db-b8cf35b6187c)

Fail2ban will automatically set up a background service after being installed. However, it is disabled by default, because some of its default settings may cause undesired effects. You can verify this by using the systemctl command:

systemctl status fail2ban.service

![Pasted image 20240428180809](https://github.com/lm3nitro/Projects/assets/55665256/852d0464-bc4f-40de-aee0-d0b51db281a8)


Configuring Fail2ban:

The fail2ban service keeps its configuration files in the /etc/fail2ban directory. There is a file with defaults called jail.conf. 

Lets create a new file called jail.local

cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

Note:

The jail.conf file contains a basic configuration that you can use as a starting point, but it may be overwritten during updates. Fail2ban uses the separate jail.local file to actually read your configuration settings.

# Restarting services:

service fail2ban restart

The attacker 's ip has been banned 

![Pasted image 20240428183041](https://github.com/lm3nitro/Projects/assets/55665256/bbe6d98e-a10f-4b43-9715-425813b4eca4)


Logs from the SSH server:

![Pasted image 20240428182854](https://github.com/lm3nitro/Projects/assets/55665256/015efbff-b8bd-468c-9a69-26ed31ecb9e4)

![Pasted image 20240428182719](https://github.com/lm3nitro/Projects/assets/55665256/a2c1f2df-5d7e-4a7d-bb1e-3849fa5e4097)


