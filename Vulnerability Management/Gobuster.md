# $${\color{blue}Gobuster}$$

![Pasted image 20240424135058](https://github.com/lm3nitro/Projects/assets/55665256/2230464c-16a1-4846-8a82-f555f1284119)

Gobuster is a versatile tool that can perform brute-force attacks on various targets, including URIs (directories and files) within websites, DNS subdomains (with wildcard support), Virtual Host names on target web servers, Open Amazon S3 buckets, Open Google Cloud buckets, and TFTP servers.

### Scope:
In this exercise, I will install Gobuster and use it to scan my web server to identify any hidden directories through the use of a wordlist. Additionally, I will review the traffic generated during the scan using Wireshark.

### Tools and Technology:

Gobuster, Linux OS, and WireShark

Lets install Gobuster!

## Getting Started:

To get started, I first updated the packages and repositories:

```
sudo apt update && sudo apt upgrade 
```

![Pasted image 20240424135247](https://github.com/lm3nitro/Projects/assets/55665256/471d412c-2623-429e-95b8-022a89d5a498)

I then installed Go:

```
sudo apt install golang-go
```

![Pasted image 20240424135336](https://github.com/lm3nitro/Projects/assets/55665256/ad40d299-1117-4efb-8559-9db0ffdb7b04)

Next, I isntalled Gobuster:

```
sudo apt install gobuster
```

![Pasted image 20240424140349](https://github.com/lm3nitro/Projects/assets/55665256/69fb77e8-0cb3-4431-bd9c-53b74c3323db)

I also made sure to install the latest version:

```
go install github.com/OJ/gobuster/v3@latest
```

![Pasted image 20240424143148](https://github.com/lm3nitro/Projects/assets/55665256/fd06096b-5888-4be9-b9ac-2a9f115ce732)

Verifying version:

```
./gobuster version
```

![Pasted image 20240424151621](https://github.com/lm3nitro/Projects/assets/55665256/307e4b0d-eb3c-41d7-9b15-af60c2cd6638)

> [!NOTE]  
> Once Go was set up, I simply ran the command go install github.com/OJ/gobuster/v3@latest to install the latest version of gobuster. The “go install” command will place the gobuster executable file in either the $GOPATH/bin or $HOME/go/bin directories.

![Pasted image 20240424143623](https://github.com/lm3nitro/Projects/assets/55665256/67a526dc-c25e-431a-844d-773ec189316d)

Getting help:

```
gobuster -h
```

![Pasted image 20240424143739](https://github.com/lm3nitro/Projects/assets/55665256/c665e9e9-90ca-4e63-a55e-15f0e591329f)

Looking at the help command, Gobuster has a few modes:

+ dir — Directory enumeration mode.
+ dns — Subdomain enumeration mode.
+ fuzz — Fuzzing mode.
+ s3 — S3 enumeration mode.
+ vhost — Vhost enumeration mode.

```
man gobuster
```

![Pasted image 20240424140600](https://github.com/lm3nitro/Projects/assets/55665256/281a41d8-9fbc-4886-9ced-1175b9fe4097)

## Downloading a Wordlist:

To download the wordlist that I will be using, I went to crackstation.net:

![Pasted image 20240424141337](https://github.com/lm3nitro/Projects/assets/55665256/b60a098e-d69b-408a-a7cf-6c88ba55711a)

This wordlist is extensive as it is 15GB in size

![Pasted image 20240424142224](https://github.com/lm3nitro/Projects/assets/55665256/b2d57d56-a1c2-4ed3-953b-00af61d773ce)

Here it is in my Downloads directory

![Pasted image 20240424144327](https://github.com/lm3nitro/Projects/assets/55665256/cee165e3-c2fc-4521-882d-ec84fd88cb34)

## Scanning 

Now that I have Gobuster installed and my wordlist, I will be scanning directories on my web server. To do this, I will be using Gobuster in directory mode.

> [!TIP]
> Gobuster's directory mode helps to look for hidden files and URL paths. This can include images, script files, and almost any file that is exposed to the internet.

Here is the command I used to run it in dir mode:

![Pasted image 20240424144440](https://github.com/lm3nitro/Projects/assets/55665256/4a6d0032-b884-4545-8835-6ff9a907512d)

For this scan I used the common.txt wordlist

```
sudo ./gobuster dir -u http://192.168.91.129 -w ~/Downloads/common.txt
```

This was the output and results of what it was able to find:

![Pasted image 20240424144151](https://github.com/lm3nitro/Projects/assets/55665256/5cf74baf-c6f9-4b49-abeb-3c317c622b8f)

## Identifying Gobuster on the Network:

Being able to identify if Gobuster or similar tools are being used on the network is important for several reasons. Knowing if they're in use helps differentiate legitimate security testing from potentially malicious activity. Secondly, Gobuster's capabilities include directory and file enumeration, brute-force attacks, and scanning for vulnerabilities, so its presence could indicate someone probing for weaknesses in the network's web services or authentication mechanisms.

While running Gobuster, I also had Wireshark running in the background to capture the traffic and see what it looks like. Here is an example packet:

![Pasted image 20240424144742](https://github.com/lm3nitro/Projects/assets/55665256/27a47d92-9b52-4749-aec2-117e1e6880e3)

I  can also see the user agent is listed as Gobuster:

![Pasted image 20240424144707](https://github.com/lm3nitro/Projects/assets/55665256/d524b8f7-35ad-4367-a2a8-ecf6e41f2127)

## Second Scan

Here I wanted to use the Gobuster with the previously downloaded wordlist that is more than 15GB in size. As you can see, the CPU is at 109%:

![Pasted image 20240424145002](https://github.com/lm3nitro/Projects/assets/55665256/494400bc-6d23-4205-93c1-10511a377396)

These are some of the directories requested by gobuster from the wordlist.txt:

![Pasted image 20240424145908](https://github.com/lm3nitro/Projects/assets/55665256/70cad1c1-efb6-49f7-8f3c-a2c7184aeda0)

While using the larger wordlist I was able to see that Gobuster was able to find 3 new hidden directories compared to the first wordlist I used which wasn't as extensive. More would have been found, but I stopped the process due to the size. 

![Pasted image 20240424150040](https://github.com/lm3nitro/Projects/assets/55665256/1f4b4262-e90d-42c2-b398-078e26f80ee4)

## Defending Against Gobuster

Gobuster is a great tool that can be used to find hidden directories, URLs, sub-domains, and S3 Buckets. However, this enables malicious hackers to use it and attack web applications. The following steps can be taken to prevent and stop brute-force attacks on web applications: 

1. Implement strong access controls and authentication mechanisms to prevent unauthorized access to sensitive directories and files.
2. Use rate limiting and IP blocking to deter brute-force attacks.
3. Regularly monitor and analyze network traffic for unusual or suspicious patterns that may indicate scanning or enumeration activities. 
5. Keep software and systems up to date with security patches and configurations to mitigate known vulnerabilities that such tools may exploit.
6. To prevent resources like S3 from being exposed on the internet, use AWS bucket policies to prevent unauthorized access.

In this exercise, I installed and configured Gobuster, then used it to scan for hidden files and paths on my web server. Through this process, I discovered three directories. It's important to periodically test devices, particularly web servers, to ensure there are no hidden or unlinked resources, such as admin pages, backup files, configuration files, or other sensitive assets, that could be vulnerable to attack.


