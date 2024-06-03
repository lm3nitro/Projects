# Part 2- SSH Brute Force Simulation Attack

This is part 2 of the project. In Part 1 I installed and configured the Wazuh management server and agents. I will be simulating a SSH brute force attack against the Linux host that has the Wazuh agent installed. After the simulation attack has been performed, I will be analyzing the traffic pertaining to the attack in both Wireshark and in the Wazuh management server. 

## Installing OpenSSH

I will first begin by installing openssh on the target host with the Wazuh agent.

```
sudo apt install openssh-server
```

![Pasted image 20240428154447](https://github.com/lm3nitro/Projects/assets/55665256/2c173ea3-5f37-4b07-8c7c-fd13f984acd5)

Verified that SSH is running:

```
sudo systemctl status ssh
```

![Pasted image 20240428154650](https://github.com/lm3nitro/Projects/assets/55665256/7a0bc0f5-1b2d-4a3f-b4d6-59e25d19367e)

Enabled SSH persistence after reboot

```
sudo systemctl enable ssh
```

![Pasted image 20240428154733](https://github.com/lm3nitro/Projects/assets/55665256/11398984-fba3-4211-b537-680b575ed4fd)

Checked for listening SSH service

```
sudo ss -tnlp
```

![Pasted image 20240428154822](https://github.com/lm3nitro/Projects/assets/55665256/b3c4e255-82c9-4e22-aaea-f2708bed854f)

## Attacking

Once SSH was installed on the target host, I then moved to the attacking host to perform the SSH brute-force attack. I will be using Hyrda to perform the attack a long with a wordlist:

![Pasted image 20240428174334](https://github.com/lm3nitro/Projects/assets/55665256/2a8b6fd7-44d9-4d20-9564-2eae56ec7f17)


+ -l specifies a username during a brute force attack.
+ -L specifies a username wordlist to be used during a brute force attack.
+ -p specifies a password during a brute force attack.
+ -P specifies a password wordlist to use during a brute force attack.
+ -t set to 4, which sets the number of parallel tasks (threads) to run.

## Analysis

Now that I have performed the attack on the target host, lets check the logs on the target server. Here we can see many login attempts. We also see the 'error: maximum authentication attempts exceeded ...'. This is an indication of a brute force attack on our server.

![Pasted image 20240428174746](https://github.com/lm3nitro/Projects/assets/55665256/5f4ae516-7dc6-4684-9060-444d360ca6b0)

While performing the brute force attack, I also had Wireshark running on the target host. Here is a look at what was captured. I was able to see the protocol SSH and many key exchanges:

![Pasted image 20240428180024](https://github.com/lm3nitro/Projects/assets/55665256/ec165731-3d5c-4459-bcc2-ef5eda43dd90)  

Next, I took a look at the Wazuh management server to see if it was able to detect the attack/malicious activity. I was able to see 548 authentication attempts and all coming from SSH authentication login attempts:

![Screenshot 2024-05-06 232548](https://github.com/lm3nitro/Projects/assets/55665256/53598b4a-e2de-43ba-adec-8d34f7c0f0dd)


![Pasted image 20240428175417](https://github.com/lm3nitro/Projects/assets/55665256/59ef1209-f19d-4e7f-ad5e-2b5fa5379ae8)

