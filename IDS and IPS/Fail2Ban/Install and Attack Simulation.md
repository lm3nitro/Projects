# Fail2Ban

<img width="418" alt="Screenshot 2024-06-02 at 1 34 19 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/4c6f7e45-9357-48e8-8df8-655b426de5cf">

Fail2Ban is an open-source intrusion prevention software that monitors server log files for intrusion attempts and other suspicious activities. 
Fail2ban, also helps to secure your server against unauthorized access attempts. It is particularly effective in reducing the risk from scripted attacks and botnets.

Here are some of the key features:
+ Log Monitoring
+ Automatic Banning
+ Configurable Filters and Actions
+ Multi-Serve Protection
+ Email Notification

### Scope:

I will install Fail2Ban on a target host and will simulate an attack using Hydra to perform a SSH brute force attack against the target host and will evaluate the effectiveness of Fail2Ban in preventing unauthorized access on a single host. I will be using Hydra to simulate malicious activity and monitoring Fail2Ban's ability to detect and block those malicious activities.

### Tools and Technology:

Fail2Ban, Linux OS, and Hydra


## Installation

First, I needed to install Fail2Ban:

```
sudo apt install fail2ban
```

![Pasted image 20240428180638](https://github.com/lm3nitro/Projects/assets/55665256/4e8c7012-cae1-4479-94db-b8cf35b6187c)


Fail2ban will automatically set up a background service after being installed. However, it is disabled by default. This is because some of its default settings may cause undesired effects. You can verify this by using the systemctl command:

```
systemctl status fail2ban.service
```

![Pasted image 20240428180809](https://github.com/lm3nitro/Projects/assets/55665256/852d0464-bc4f-40de-aee0-d0b51db281a8)

## Configuring Fail2ban:

Once I install Fail2Ban, I will need to get it configured. The fail2ban service keeps its configuration files in the /etc/fail2ban directory. There is a file with defaults called jail.conf. I created a new file called jail.local

```
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

>#### Note: The jail.conf file contains a basic configuration that you can use as a starting point, but it may be overwritten during updates. Fail2ban uses the separate jail.local file to actually read your configuration settings.

After making any changes, I needed to restart Fail2Ban:

```
service fail2ban restart
```

## Attacking host

I then jumped over to the attacking host and used Hydra and the rockyou wordlist to simulate a SSH Brute Force attack on the target host where Fail2Ban was just installed. As seen in the screenshot below, the connection was refused:

![Pasted image 20240428183041](https://github.com/lm3nitro/Projects/assets/55665256/bbe6d98e-a10f-4b43-9715-425813b4eca4)


## Monitoring and Analysis

I went back to my target host to evaluate the logs from the SSH server:

![Pasted image 20240428182854](https://github.com/lm3nitro/Projects/assets/55665256/015efbff-b8bd-468c-9a69-26ed31ecb9e4)

I also check iptables and can see that an entry was added by Fail2Ban to reject/block the IP address from the attacking host:

![Pasted image 20240428182719](https://github.com/lm3nitro/Projects/assets/55665256/a2c1f2df-5d7e-4a7d-bb1e-3849fa5e4097)

### Summary:

Doing this project allowed me to test the effectiveness that Fail2Ban has in detecting and preventing a SSH brute force attack by using Hydra for a simulated attack on the target host. I was able to get experience in configuring Fail2Ban, monitoring logs, and analyzing its automated responses to malicious activities. The findings offer valuable insights into enhancing server security. 



