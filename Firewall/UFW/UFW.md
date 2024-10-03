# UFW:

The Uncomplicated Firewall (UFW) is a user-friendly front-end for managing iptables, designed primarily for Linux systems. It simplifies the process of configuring a firewall while providing powerful features for managing network security. Some of the features include:

+ Predefined Application Profiles
+ IPv4 and IPv6 Support
+ Logging Capabilities
+ Simplictic/User friendly

![Pasted image 20240510212020](https://github.com/lm3nitro/Projects/assets/55665256/c9ad8d0f-e9b1-40e8-8d72-3148bd565f2d)

### Scope:

I will be configuring and testing UFW on an Ubuntu VM which will consist of setting up rules to allow and deny specific traffic, enabling the firewall, and removing policies. 

### Tools and Technology:

Ubuntu and UFW 

## Getting Started

UFW typically comes pre-installed and is easily available in the repositories. To get started I first searched for it:

```
apt search ufw
```

![Pasted image 20240510205540](https://github.com/lm3nitro/Projects/assets/55665256/e27dac59-67f3-4ae8-9fbb-2fbfba3b9486)

Next I installed it, as seen below a new package was installed:

```
apt isntall ufw
```

![Pasted image 20240510205658](https://github.com/lm3nitro/Projects/assets/55665256/ed88de24-d802-43b0-8170-184ee32c05bf)

To view the status of ufw,type:

```
ufw status
```

![Pasted image 20240510205739](https://github.com/lm3nitro/Projects/assets/55665256/a618e2fb-11aa-46b6-b37e-b1e92a4ad159)

> [!TIP]
> The default policy firewall works great for both servers and desktop versions. It is always best practice to close all ports on the server and open only required ports individually.

## Configuration

I first blocked all incoming connections and allowed only outgoing connections. TO do this, I used the following commands:

```
ufw default allow outgoing
ufw default deny incoming
```

![Pasted image 20240510205947](https://github.com/lm3nitro/Projects/assets/55665256/3ea4e051-ed8c-4e17-b6f9-ad2e396689cf)

For testing, I will be opening and allowing SSH TCP port 22 connections:

```
ufw allow ssh
```

![Pasted image 20240510210023](https://github.com/lm3nitro/Projects/assets/55665256/94337d41-c831-4b77-919f-e1090d9443b8)

I also opened a random TCP port:

```
ufw allow 12345/tcp
```

![Pasted image 20240510210331](https://github.com/lm3nitro/Projects/assets/55665256/41ffa355-bde2-4e56-8ca1-94bdc21bf53d)


Now that the SSH port 22 has been allowed, I narrowed the policy down to only allow SSH access from a static IP address, in this case 10.10.100.1. I will be allowing this to the Ubuntu server on 10.10.100.105:

```
ufw allow proto tcp from 10.10.100.1 to 10.10.100.105 port 22
```

![Pasted image 20240510210411](https://github.com/lm3nitro/Projects/assets/55665256/a1b8c0b3-05d6-42ca-a812-dfc4f0fd0626)

After having the rules in place, I turned on the Firewall:

```
ufw enable
```

![Pasted image 20240510210523](https://github.com/lm3nitro/Projects/assets/55665256/0ecf0a49-b747-47b1-867d-59e6b9296684)


Cnce UFW enabled, it runs across system reboots too. We can verify that easily as follows using the systemctl command:

![Pasted image 20240510210600](https://github.com/lm3nitro/Projects/assets/55665256/c374f16e-06e7-4f1a-80f2-e9c682cc2e0d)

Also important to note, the firewall is currently configured in a way that if the system were to reboot, the firewall would be disabled upon startup. I rebooted the server, so the firewall is currently disabled:

![Pasted image 20240510210635](https://github.com/lm3nitro/Projects/assets/55665256/7f3e7cf7-d3e4-4599-9f04-1314fefc8109)

You can also open/allow specific incoming connections/ports with a description:

```
ufw allow 80/tcp comment 'lm3nitro Ngnix'
```

![Pasted image 20240510210727](https://github.com/lm3nitro/Projects/assets/55665256/f263f945-dc0f-447e-bdb3-8e768ddb9df8)

```
ufw allow 443/tcp comment 'HTTPS lm3nitro traffic'
```

![Pasted image 20240510210816](https://github.com/lm3nitro/Projects/assets/55665256/357160d6-22e3-4596-b6a3-652cbc6f2291)

After adding the rules and their descriptions, I enabled the firewall and verified the status and the rules that I had created:

```
ufw enable
ufw status
```

![Pasted image 20240510210928](https://github.com/lm3nitro/Projects/assets/55665256/1c70a377-8724-48f4-9f68-04eabcbb440e)

## Allowing port ranges

If you do not wish to enable ports one by one, but instead a full range, you can do so by usinbg port ranges. As an example, if you want to alow tcp and udp ports 100 to 500, you would use the following syntax:

```
ufw allow 100:500/tcp
ufw allow 100:500/udp
```

![Pasted image 20240510211128](https://github.com/lm3nitro/Projects/assets/55665256/5073ca0d-7281-48b6-934f-e62a1f730775)

To allow all connections from a specific IP, the following command can be used, I will be using the IP 10.10.100.10

```
ufw allow from 10.10.100.10
```

![Pasted image 20240510211225](https://github.com/lm3nitro/Projects/assets/55665256/6efe456f-8491-4906-8162-ff5f3e39519c)


## Delete UFW Rules

Next, if you wish to delete/remove a rule, you can first list out the rules with their respective number. THis wil list out the rules with the line number that can be used to reference the rule you no longer want:

```
ufw status numbered
```

![Pasted image 20240510211337](https://github.com/lm3nitro/Projects/assets/55665256/b2140e6e-5710-4eea-817c-7952794b9b0a)

To delete 7th rule type the command:

```
ufw delete 7
```

Now that I deleted rule number 7 regarding the UDP port ranges, I went back and verified:

```
ufw status numbered
```

It was removed:

![Pasted image 20240510211515](https://github.com/lm3nitro/Projects/assets/55665256/bab8a775-f5a0-4f2d-a4c4-05ac5d660845)

## Reset UFW:

If you wish to reset the firewall back to its default settings, the following command can be used:

```
ufw reset
```

Aftger typing the command, I reloaded the ufw:

```
ufw reload
```

![Pasted image 20240510211640](https://github.com/lm3nitro/Projects/assets/55665256/f1ad9acb-2b6e-4120-a23a-1507fdf49236)


## Firewall logs:

The following command allows you to view the UFW log file. This log contains records of all firewall activities, including allowed and denied connection attempts. It can also provide valuable insights into network traffic and potential security incidents. 

```
more /var/log/ufw.log
```

![Pasted image 20240510211804](https://github.com/lm3nitro/Projects/assets/55665256/f3676686-3a84-499a-a6a7-7c14a20b963f)


Another command to view the latest entries in the log is:

```
tail -f /var/log/ufw.log
```

![Pasted image 20240510211827](https://github.com/lm3nitro/Projects/assets/55665256/99f69323-bab0-46fc-87bb-31384011c8df)

The follwiong command be used to display a list of all the ports and services that are currently allowed through the firewall and are actively listening for incoming connections:

```
ufw show listening
```

![Pasted image 20240510211855](https://github.com/lm3nitro/Projects/assets/55665256/f26503e2-8f41-4722-a8ce-bd2706bcd201)

This command below shows the list currently added UFW rules. Since we removed everything and reloaded, this is currently showing as empty:

```
ufw show added
```

![Pasted image 20240510211927](https://github.com/lm3nitro/Projects/assets/55665256/10d64a64-cdab-4cbc-906c-5f2dd04dd511)

### Summary:

This project was more of a walkthrough to demonstrate the configuration of the UFW. By implementing a default deny policy and allowing only specific services, UFW contributes to device hardening, minimizing the attack surface and protecting against unauthorized access. Implementing device harding measure ensures that only necessary services and ports are active, minimizing vulnerabilities that attackers could exploit. This proactive approach helps safeguard sensitive data, maintain system integrity, and enhance overall cybersecurity posture.

Additionally, becuase UFW is open source and lightweight, it ensures minimal impact on system performance, making it a practical choice for budget-conscious users. Overall, a great firewall solution. 



