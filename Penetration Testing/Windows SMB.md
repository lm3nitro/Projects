# Windows SMB

<img width="300" alt="Screenshot 2024-10-06 at 1 56 51 PM" src="https://github.com/user-attachments/assets/8e9b8914-9dd1-4b36-9e95-fc864cbba124">

SMB (Server Message Block) is a network file sharing protocol that allows applications, users, or devices to access and manage files on remote servers. In Windows, SMB is primarily used to share files, printers, and serial ports, and it enables seamless access to network resources, making it a core component of Windows networking.

### Scope:

In this exercise, I will scan a Windows host using Nmap to identify open ports and vulnerable services. I will then verify the vulnerability using Tenable, and then exploit it with Metasploit to gain access. Once inside, I will extract password hashes and use John the Ripper to crack them, allowing me to log in as an authenticated user. This simulates a full penetration test, from discovery to exploitation and password cracking.

### Tools and Technology:

Windows 7, SMB, Wireshark, John the Ripper, Mimikatz, Hashcat, Linux, Metasploit, Remote Desktop, and Tenable

## Getting Started

To get started, I used my Linux VM and ran a nmap scan against a Windows 7 VM in my network on 192.168.91.137:

![Pasted image 20240430150728](https://github.com/lm3nitro/Projects/assets/55665256/4c844034-1d00-45f5-aece-fc84dd32fecc)

I can see that ports 135, 139, 445 are open. Knowing that these are open, I then ran all the SMB scripts to see if I could find something. Below are all the NSE scripts related to SMB:

![Pasted image 20240430131406](https://github.com/lm3nitro/Projects/assets/55665256/dcd0c45d-f649-4dc7-9529-23ff29195fa4)

Example of using specify NSE script:

![Pasted image 20240430131631](https://github.com/lm3nitro/Projects/assets/55665256/e45b47fa-8ac3-40f4-857c-a636e8a8c1e0)

Running all the scripts against the host:

![Pasted image 20240430130929](https://github.com/lm3nitro/Projects/assets/55665256/7a02251b-f8e6-4ddb-8755-b90fca42c2fb)

Zoomed in to see the vulnerability better:

![Pasted image 20240430131124](https://github.com/lm3nitro/Projects/assets/55665256/2394fd85-53da-4d6c-a2f1-3fc45a1272fd)

## Nessus

Using Nmap above, I was able to see that the host was vulnerable to CVE-2017-0143. Knowing this, I wanted to further confirm and used tenable to run a scan against the host and see what it could find. Tenable was able to find 2 critical vulernabilities and 1 high:

![Pasted image 20240430125818](https://github.com/lm3nitro/Projects/assets/55665256/d8eb4157-fced-4087-a586-b6e431edcc02)

Looking further into the vulnerability using tenable, this matches what we found above with Nmap:

![Pasted image 20240430125946](https://github.com/lm3nitro/Projects/assets/55665256/4d24cee6-82ae-4b9f-98d0-1ce40afaf672)

## Metasploit:

Now that I know that this host is vulernable to SMB, I will use Metasploit to execute the exploit against this vulnerability:

![Pasted image 20240430132250](https://github.com/lm3nitro/Projects/assets/55665256/85d94b75-c48b-4b44-86a4-4ffb72892a54)

Searching for the exploit:

![Pasted image 20240430132540](https://github.com/lm3nitro/Projects/assets/55665256/9f23abb8-0ccb-4f2c-8f7d-13f31408e29b)

Using an auxiliary scanner:

![Pasted image 20240430133026](https://github.com/lm3nitro/Projects/assets/55665256/9156e165-f04d-465a-ab12-29ded9b478e6)

Executing the exploit:

![Pasted image 20240430133353](https://github.com/lm3nitro/Projects/assets/55665256/a3893b46-0d83-4071-ac52-b7643cd28f40)

HOST and LHOST are parameters that define network targets and your own system's settings. RHOST is the target system and LHOST is my Linux machine (attacking system):

set RHOST 192.168.91.137
set LHOST 192.168.91.134

![Pasted image 20240430140159](https://github.com/lm3nitro/Projects/assets/55665256/1f0c4455-65d3-4bc3-9aa3-1e08199137c3)

As seen in the screenshot above, the exploit was successful. 

## Wireshark

Using Wireshark while the exploit was running, allowed me to capture and analyze the network traffic, helping to visualize how the exploit behaves behind the scenes. It showed details like packet exchanges, payload delivery, and vulnerabilities being targeted. This helped me to better understand the exploit's impact. 

Below is the traffic capture using Wireshark during the exploit:

![Pasted image 20240430145719](https://github.com/lm3nitro/Projects/assets/55665256/7fea045a-b95f-4977-81d7-9109ec58315a)

Looking further into the traffic related to the exploit:

![Pasted image 20240430145921](https://github.com/lm3nitro/Projects/assets/55665256/c981eb3c-ef69-474d-a44b-4eda2925c46b)

## Remote Session

The exploit successfully compromised the target which created a reverse shell. In the screenshot below, I was given the option to upgrade Meterpreter shell, this is optional and not needed. 

![Pasted image 20240430141151](https://github.com/lm3nitro/Projects/assets/55665256/7da1055c-2225-4b7e-a998-a2129f24fdd6)

I went back into the running session:

![Pasted image 20240430141305](https://github.com/lm3nitro/Projects/assets/55665256/0660bea4-2b41-42e8-917c-9c40fbbd00a7)

Taking a look at the directories on the Win 7 host:

![Pasted image 20240430141716](https://github.com/lm3nitro/Projects/assets/55665256/f527d86a-2b9a-4dae-9334-cc0327e423d3)

I decided to go into the user directory and can see a user by the name lm3nitro:

![Pasted image 20240430142147](https://github.com/lm3nitro/Projects/assets/55665256/6fe119f1-e1fb-4311-91ca-acb673fbffa2)

I then created a directory with this sanme users name under Desktop:

![Pasted image 20240430142553](https://github.com/lm3nitro/Projects/assets/55665256/86326d78-c2cb-4386-be07-6af2699bf3a3)

On the windows 7 box, I was able to see that the directory called lm3nitro was indeed created:

![Pasted image 20240430142453](https://github.com/lm3nitro/Projects/assets/55665256/b3a48d82-9cee-40f1-b65e-eaf8b5024d02)

I was also able to see the reverse shell connection that had been established. 

## Dumping the hash and cracking it

Now that I know that there is a user by the name lm3nitro, I wanted to crack the user's password. To do this, I first used `hashdump`:

![Pasted image 20240430145228](https://github.com/lm3nitro/Projects/assets/55665256/2649f103-281e-49e6-a44a-7be91ddba771)

With the provided information from `hashdump`, I created a file to crack it with John the Ripper:

![Pasted image 20240430145322](https://github.com/lm3nitro/Projects/assets/55665256/dfab06df-d70f-4b52-b697-7cd559552dd7)

## John the Ripper

![Pasted image 20240502094456](https://github.com/lm3nitro/Projects/assets/55665256/50b529a5-68e1-4a2f-8487-dfdacd256f62)

Once I had the file saved, I then used John the Ripper along with a wordlist and the previos found hash:

![Pasted image 20240430145055](https://github.com/lm3nitro/Projects/assets/55665256/9e1d6c18-17b8-47aa-bdcb-55de62f518d9)

## Hashcat

Another way to do this is by using Hashcat:

![Pasted image 20240506102724](https://github.com/lm3nitro/Projects/assets/55665256/1027efb9-96a0-44a8-8483-719bb9c9319f)

Cracking the password:

![Pasted image 20240506104207](https://github.com/lm3nitro/Projects/assets/55665256/3b8659c0-2377-45a8-a40f-00cfa3b7c248)

## Mimikatz

Mimikatz can also be used:
![Pasted image 20240506105845](https://github.com/lm3nitro/Projects/assets/55665256/5d1c8c7a-5e20-4e13-b8e0-c4a6debed536)

Now we can go to see all the credentials:

![Pasted image 20240506111530](https://github.com/lm3nitro/Projects/assets/55665256/8f6cf77c-991d-4594-9efb-ea7d2242d4ce)

All the credentials, including the username and password for lm3nitro:

![Pasted image 20240506111623](https://github.com/lm3nitro/Projects/assets/55665256/b959c579-700e-482a-beb8-5ae3f6ff927c)

## Remote Desktop

Now that I have the credentials, I use Remote Desktop to login:

![Pasted image 20240430151932](https://github.com/lm3nitro/Projects/assets/55665256/199e2404-476d-4db3-b72d-1063a559af6a)

I successfully logged in as lm3nitro:

![Pasted image 20240430151350](https://github.com/lm3nitro/Projects/assets/55665256/3739ff7a-8c28-4ddc-9661-8e45c71d2d87)

Listing the established connections:

![Pasted image 20240430152107](https://github.com/lm3nitro/Projects/assets/55665256/b098f300-75ec-42e5-84e2-98f6717b1049)

### Summary:

In this exercise, I was able to identify a vulnerability, exploit it, and understand the implications of a successful attack, as well as techniques for credential extraction and network traffic analysis. This allowed me to better understand the entire penetration testing process, from reconnaissance to exploitation and post-exploitation. 

This exercise also demonstrated the necessity of maintaining systems that are patched and up-to-date as a way to mitigate vulnerabilities and reduce the risk and attack surface of exploitation by adversaries.
