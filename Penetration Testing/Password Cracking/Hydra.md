# $${\color{lightgreen}Hydra}$$

Hydra is a brute-forcing tool that helps penetration testers and ethical hackers crack passwords of different network services. Hydra can perform rapid dictionary attacks against more than 50 protocols. This includes telnet, FTP, HTTP, HTTPS, SMB, databases, and other services.

![Pasted image 20240425102445](https://github.com/lm3nitro/Projects/assets/55665256/09f19037-82c2-4b5f-902f-7f006f8bac69)

A brute force attack is a hacking method that uses trial and error u crack passwords, login credentials, and encryption keys. It is a simple yet reliable tactic for gaining unauthorized access to individual accounts and organizations’ systems and networks. When using the brute force method, the adversary tries multiple usernames and passwords, often using a computer to test a wide range of combinations, until they find the correct login information. 

### Scope:

Install Hydra and use it in combination with a wordlist to perform a dictionary attack into a FTP server. Analyze the behavior from both the victim and attacker perspective. 

### Tools/Technology:

Hydra, Linux OS, and Wireshark

## Installation:

To get started, I will install Hydra. This can be done through default repository:

```
sudo apt install hydra-gtk
```

![Pasted image 20240425102740](https://github.com/lm3nitro/Projects/assets/55665256/571afd57-9b44-46e3-a298-935b7d6aadd7)

You can also install Hydra through GitHub:

>#### Note: This method allows you to install the latest update of Hydra directly from the source code. 

```
git clone https://github.com/vanhauser-thc/thc-hydra.git
```

![Pasted image 20240425103054](https://github.com/lm3nitro/Projects/assets/55665256/f06aec44-a7fc-429d-881c-40e23fe5b9ed)

Once complete, navigate from the current directory to the cloned directory. Once there, configure the cloned directory:

```
sudo ./configure
```

![Pasted image 20240425103231](https://github.com/lm3nitro/Projects/assets/55665256/6c2e8e51-89e7-4436-ac12-343dd9f03df5)

After that has completes use the make and the make install command:

```
sudo make
```

![Pasted image 20240425103415](https://github.com/lm3nitro/Projects/assets/55665256/94581dab-8b5f-43b5-894a-b481004a8aa4)

```
sudo make install 
```

![Pasted image 20240425103910](https://github.com/lm3nitro/Projects/assets/55665256/0dd3d797-782f-403f-855d-328f475e3e5a)

>#### Note: If you need help, you can always use the following command to get more information
```
hydra -h
```

![Pasted image 20240425104049](https://github.com/lm3nitro/Projects/assets/55665256/47a11fda-374a-4f1a-a1f7-57d22745f3bc)

## Downloading a wordlist:

Rockyou.txt is a list comprised of over 14 million plaintext passwords from the 2009 RockYou hack. Passwords from this wordlist are commonly used in CTF and penetration challenges. Once downloaded, extract the tar.gz file:

```
tar -xvzf 
```

![Pasted image 20240425105156](https://github.com/lm3nitro/Projects/assets/55665256/cfe2b4bc-15b0-4cc6-be00-0c3ecdc2d1a3)

## Dictionary Attack

Dictionary Attack: If a more sophisticated approach is warranted, a dictionary attack is often used. Here, the attacker is more discerning, wielding a list of common, often used passwords a proverbial dictionary of likely keys. It's akin to someone trying out the most common door codes in a building, hoping that simplicity will lead to success.

Here I will use Hydra with the downloaded rockyou.txt previously downloaded against the ftp protocol:

```
sudo Hydra -L userlist.txt -P rockyou.txt 192.168.91.129 ftp
```
![Pasted image 20240425113729](https://github.com/lm3nitro/Projects/assets/55665256/6636406e-5606-41d9-b5e5-d258b0f132d3)

## Victim FTP Server

I wanted to jump on the victim ftp server and see how the dictionary attack can be identified. When analyzing, I looked at the auth.log on the FTP server to identify password attacks: 

```
tail -f /var/log/auth.log
```

![Pasted image 20240425121329](https://github.com/lm3nitro/Projects/assets/55665256/10115dbb-125a-4523-841f-aa71b27b11e1)

Now, I will filter for code responses from the FTP server in Wireshark:

![Pasted image 20240425111907](https://github.com/lm3nitro/Projects/assets/55665256/e352fe63-5655-437b-9a56-0e5a4ef7022f)

Looking at the traffic, I can see all the password attempts:

![Pasted image 20240425112144](https://github.com/lm3nitro/Projects/assets/55665256/a5b493a4-6b91-4c69-94cc-28d9de3947e0)

When following the stream, I can get more details:

![Pasted image 20240425112047](https://github.com/lm3nitro/Projects/assets/55665256/62fcfb08-c01a-4a9a-98d0-9234d0551f5e)

## Attacker:

Going back to our attacker host, it looks like Hydra was able to find the username and password! 

![Pasted image 20240425112822](https://github.com/lm3nitro/Projects/assets/55665256/16e25ebe-5915-40e8-a028-c644dffe7635)

I also had Wireshark running on the attacking host in order to view the traffic from both perspectives. Let's filter for a successfully login:  

![Pasted image 20240425113419](https://github.com/lm3nitro/Projects/assets/55665256/bf46f1b2-0ac2-4ba7-b542-7193b5d49db8)

## Summary:

Here I can see that the FTP server username and password were both found using Hydra. To help protect against dictionary attacks, there are several strategies that can be implemented:

+ Strong Password Policy
+ Account Lockout Policy
+ Rate Limiting
+ MFA
+ User Education

Having these security measures in place can help protect against this type of attack. 

