# Part 3- RDP Brute Force Simulation Attack

<img width="355" alt="Screenshot 2024-06-02 at 11 10 18 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d62ed72b-d3dd-4a6f-a8e8-e35d60e06867">

This is part 3 of the project. In Part 1, I installed and configured the Wazuh management server and agents. In Part 2 I performed a simuated SSH brute force attack on the Linux server with the Wazuh agent installed. Here, I will be simulating a RDP brute force attack against the Win 10 host that has the Wazuh agent installed. I will be analyzing the traffic pertaining to the attack in both Wireshark and in the Wazuh management server.

## Normal Traffic

To start, I went ahead and tried to login to the target host 3 times, purposefully typing in the wrong password in order to see what a failed login attempt would look like from a user that mistakenly forgot or mistyped their password. 

![Pasted image 20240428142640](https://github.com/lm3nitro/Projects/assets/55665256/756b303f-258b-4839-9d0d-7560158c79f1)

When checking the Wazuh management server, I can see that there are only 3 attempts which can be seen as normal if this was coming from a user. Seeing only 3 login attempts is not too alarming:

![Pasted image 20240428142840](https://github.com/lm3nitro/Projects/assets/55665256/7e849890-8afe-4a05-8615-a7e379fab602)

Taking a deeper look at the security events related to the failed attempts, we can see the details:

![Pasted image 20240428143019](https://github.com/lm3nitro/Projects/assets/55665256/549276f5-6095-46da-9194-f2e9d878c1c6)

Now that I know what a failed login attempt would look like under normal circumstances, I will start with the RDP simulation attack to see the difference in logging. Understanding what normal traffic looks like is crucial becuase it helps to establish a baseline for detecting anomolies, reduces false positives, and it enhances the accuracy of security tools. This is also very important becuase it helps to quickly identify and respond to malicious activity and distinguishing it from legitimate behavior. 

## RDP password attack:

To get started, I enabled RDP on the Win 10 host.

![Pasted image 20240428144107](https://github.com/lm3nitro/Projects/assets/55665256/e315122f-f920-49e1-926e-e052acbca0a0)

![Pasted image 20240428201943](https://github.com/lm3nitro/Projects/assets/55665256/05ef5fb7-64ed-41d4-87ec-013cff585bbb)

![Pasted image 20240428202103](https://github.com/lm3nitro/Projects/assets/55665256/dce2e6e9-701c-4cdc-b97d-b1df5e5dabec)


# Installing crowbar:

![Pasted image 20240428144651](https://github.com/lm3nitro/Projects/assets/55665256/4ca9c749-147b-4a54-b3fa-170ed502f71e)

# Getting help:

![Pasted image 20240428145709](https://github.com/lm3nitro/Projects/assets/55665256/f016996a-6f49-462e-98c1-4f9fdff30280)


# Recon with Nmap:

![Pasted image 20240428145631](https://github.com/lm3nitro/Projects/assets/55665256/f1235d15-b841-40e8-84cf-9f10e9defcd3)


# Lunching a password attack

![Pasted image 20240428150754](https://github.com/lm3nitro/Projects/assets/55665256/ffc2758c-5c8a-403e-83d2-bddf2c6acedc)

# RDP traffic analysis:

This looks like a password guessing attack over the network, we can see many users attempts 

![Pasted image 20240428152359](https://github.com/lm3nitro/Projects/assets/55665256/6a7f54a3-411d-4f6f-9935-481250354001)


# More information about the attacker:

![Pasted image 20240428152506](https://github.com/lm3nitro/Projects/assets/55665256/06466c9f-2b75-48cd-84e7-8f7fd47a043f)

# Looking at the Wazuh logs : 

![Pasted image 20240428150654](https://github.com/lm3nitro/Projects/assets/55665256/a8d533d4-4734-4f3d-8e6b-6315a6157193)

# Alerts:

![Pasted image 20240428150538](https://github.com/lm3nitro/Projects/assets/55665256/d061c845-fdc6-4ddc-8713-04ce7ef57d35)

# Json Logs:

![Pasted image 20240428151816](https://github.com/lm3nitro/Projects/assets/55665256/720dba89-f0d5-43bb-8afe-21c092bf776a)

# Rule information:

![Pasted image 20240428151913](https://github.com/lm3nitro/Projects/assets/55665256/3eb13198-6d9c-4d0e-aee0-7f6cdeb5f646)



