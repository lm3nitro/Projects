# $${\color{darkblue}ZAP}$$

![Pasted image 20240424161116](https://github.com/lm3nitro/Projects/assets/55665256/b7946686-0ee5-4730-b63a-dfcab2af58e3)

ZAP (Zed Attack Proxy) is what is known as a “man-in-the-middle proxy”. It stands between the tester's browser and the web application so that it can intercept and inspect messages sent between the browser and the web application, modify the contents if needed, and then forward those packets onto the destination. By intercepting and modifying the traffic between the browser and the web application, ZAP helps to test how the application behaves under different scenarios and conditions. Below is an illustration of what this looks like:

![Pasted image 20240424161332](https://github.com/lm3nitro/Projects/assets/55665256/491fb004-c9f1-4ef8-b5c5-d37c1cf9ce5c)

### Scope:
In this project, I will be installing ZAP and using it to conduct an automatic scan against a target host that running Linux and that has Apache2 runing. I will be using ZAP to scan the web server and see what vulnerabilites are present if any, and also will be taking a look on the server side to see what the scan looks like while running Wireshark to capture the traffic initiated from the scan. This will help to identify vulnerability scans against a host on the network.  

### Tools:
Linux OS, ZAP, Apache2, and Wireshark

## Downloading ZAP:

To begin, I needed to download ZAP. I downloaded it via the following URL: https://www.zaproxy.org/download/

![Pasted image 20240424161457](https://github.com/lm3nitro/Projects/assets/55665256/0ed8a041-911a-48e8-8a8f-dfec01e18103)

After choosing the OS that will be used, it provide the commands needed to get it installed:

![Pasted image 20240424161623](https://github.com/lm3nitro/Projects/assets/55665256/a957740d-aa71-44a3-a29f-0ac412d94cf9)

As part of the installation, I will be using Curl, so I to get that installed first before proceeding:
```
sudo apt install curl
```
![Pasted image 20240424161805](https://github.com/lm3nitro/Projects/assets/55665256/c6f78d5b-7d67-49e6-8c64-7564ae1b853f)

Now I can add the repo:

![Pasted image 20240424161827](https://github.com/lm3nitro/Projects/assets/55665256/80cb840b-a942-4602-80e8-8c9e3369aa96)

I can also add the key:

![Pasted image 20240424161915](https://github.com/lm3nitro/Projects/assets/55665256/5d75ab7e-bbd4-48da-8bf8-c718bfad17b2)

Next, let's update the packages and install ZAP:

![Pasted image 20240424162018](https://github.com/lm3nitro/Projects/assets/55665256/1370b8f3-4aeb-4375-bd20-7f83e911c97e)

During the isntallation, I noticed that the current host where I am installing ZAP has an older version of Java. Zap won't work with old version.

![Pasted image 20240424162454](https://github.com/lm3nitro/Projects/assets/55665256/8d2e8ab3-13af-4ea7-8ff9-9fadbe47a9e8)

>#### Installing the Default JRE/JDK : One option for installing Java is to use the version packaged with Ubuntu. By default, Ubuntu 22.04 includes Open JDK 11, which is an open-source variant of the JRE and JDK.

```
sudo apt install default-jre
```

![Pasted image 20240424162524](https://github.com/lm3nitro/Projects/assets/55665256/83bc0f1f-2c67-46b9-ba3d-086f3ccd3b5e)

Verify Java:

```
java -version
```

![Pasted image 20240424162639](https://github.com/lm3nitro/Projects/assets/55665256/c1eda971-a207-43bd-ae1a-76716087181f)

## Running ZAP:

Now that I have it installed, I was able to run it using the following command:

```
sudo owasp-zap
```

![Pasted image 20240424162819](https://github.com/lm3nitro/Projects/assets/55665256/4335aa25-3afc-4b40-9a63-9f822338751f)

I was then presented with the option to persist the current session. I selected *No* and then *Start*

![Pasted image 20240424162852](https://github.com/lm3nitro/Projects/assets/55665256/e4c3b1eb-a59e-4785-8066-42c2f734f635)

Once it was up and running, this is what I was presented with. To start, I will run an autmated scan against my web server running Apache2:

![Pasted image 20240424162934](https://github.com/lm3nitro/Projects/assets/55665256/03cca604-3de9-4d92-8325-78bf723b6aba)

After selecting *Automated Scan*, I was presented with a dialog box to enter the details of the web server. I then selected the *Attack* option:

![Pasted image 20240424163640](https://github.com/lm3nitro/Projects/assets/55665256/ef5e0084-ff93-48df-996f-655d784e8cc0)

Once I selected *Attack*, the scan automatically started running. Below are the URIs and the codes:

![Pasted image 20240424163754](https://github.com/lm3nitro/Projects/assets/55665256/2f46534b-4833-4a80-9b03-a00c9b67143f)

I selected the icon highlighted below for more details of the scan in progress. This will open a seperate dialog box with more information:

![Pasted image 20240424164232](https://github.com/lm3nitro/Projects/assets/55665256/2f7e80bd-d654-4c7a-a5f3-a2f04fd09bf8)

Now that the scan is running and I wanted to jump to the web server and see the scan from the victims perspective. The graph below was seen in Wireshark:

![Pasted image 20240424181748](https://github.com/lm3nitro/Projects/assets/55665256/35ba6941-4847-4baa-8534-0c832a432827)

I was able to see the list of all the request URIs:

![Pasted image 20240424182353](https://github.com/lm3nitro/Projects/assets/55665256/20dd739f-b7f0-4bd2-b66c-157a06b42238)

I also wanted to analyze the logs on the server itself to see what information the logs are able to provide about the current scan that is running. Especially since there was a lot of directory traversing seen in the traffic. 

This is the output from the Apache2 error.log:

![Pasted image 20240424165406](https://github.com/lm3nitro/Projects/assets/55665256/94ec6205-151d-4fd3-bc82-a0b7a496a328)

This is the output from Apache2 access.log:

![Pasted image 20240424165527](https://github.com/lm3nitro/Projects/assets/55665256/ce5abbc3-fc2f-4b43-b967-ab040dd9951d)

Based on the information provided in the access.log about the user agent, I wanted to go further and research it to see what information it could provide. 

When researching the user agent, I could see that this was not normal behavior and that it is not recognized as any OS/platform. 

![Pasted image 20240424170823](https://github.com/lm3nitro/Projects/assets/55665256/a6816fc2-e022-47ac-ae7e-467c7fbec39d)

Going back to the ZAP dashboard, I can see that ZAP was able to detect some web vulnerabilities on the server regarding Path Traversal:

![zap1](https://github.com/lm3nitro/Projects/assets/55665256/e26173c4-590c-4d7f-b63e-58b7aa791958)

I expanded to take a closer look into the alert. It also provides resources for more information regarding the vulnerability it found:

![zap2](https://github.com/lm3nitro/Projects/assets/55665256/23cf1390-2def-4949-9381-b86cc2a2323f)

I followed the mitre link provided above to get more information behind Path Traversal:

![zap3](https://github.com/lm3nitro/Projects/assets/55665256/015482ae-0bf4-4076-a416-70e601999bf9)

## Summary:

I was able to install ZAP and run an automated scan again a web server running Apache2. I was also able to analyze the scan from the victim perspective (web server). This is important and beneficial because it allowed me to see what the scan looks like in Wireshark from the web server side and be able to differentiate normal from abnormal traffic. ZAP also offers different features outside of the automated scan:

+ Manual Testing
+ Fuzzing
+ Spidering
+ Reporting, and much more

In using ZAP, I was able to identify vulnerabilities on the target server along with weaknesses such as CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal'). There are a number of ways to protect against this weakness:

+ Input Validation and Sanitation
+ Whitelist Allowed Directories and Files
+ Avoid Direct User Input in File Paths
+ Use Secure File API's
+ Least Priveledge Principle

Overall, I think this is a great tool to gain insight into web applications/servers in order to identify vulernabilities and weaknesses. 
