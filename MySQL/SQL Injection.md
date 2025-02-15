# SQL Injection Vulnerability

### Introduction:

![Pasted image 20250124114817](https://github.com/user-attachments/assets/b12da33f-3219-4b70-bad6-0e6249041620)

SQL injection is an attack where malicious SQL code is inserted into an input field to access or manipulate information in the database that is not meant to be exposed. Attackers can use SQL injection to view, modify, or delete data, bypass authentication, or even execute administrative operations on the database. SQL injection attacks can have serious consequences and can be used on any database, however, web servers are usually the most common targets. 

### Scenario:

I will be performing an SQL injection attack using a VM running Metasploitable 2 which includes DVWA that makes the target vulnerable to SQL injection. I also set up the Burp Suite proxy and will use Wireshark to capture all the traffic and data being transmitted while performing the SQL injection attack. The traffic analyzed will then be used to create Snort detection rules to detect such activity. 

### Tools and Technology:

Snort, Wireshark, Tshark, Metasploitable (Linux), and Burp Suite

### Network diagram:

![Pasted image 20250124120116](https://github.com/user-attachments/assets/8d9f584e-95dd-4250-b97c-58ffceb35e78)

## Getting Started:

To get started I will be using Metasploitable2 (Linux). Metasploitable is an intentionally vulnerable Linux virtual machine. This VM can be used to conduct security training, test security tools, and practice common penetration testing techniques.

Below is the link:

```
https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
```

![Pasted image 20250124115058](https://github.com/user-attachments/assets/69c9b4d1-da09-4cfb-9ff6-e4825730f07a)

More information: https://docs.rapid7.com/metasploit/metasploitable-2/

Once downloaded, I set up the VM in my VMWare Workstation:

![Pasted image 20250124115425](https://github.com/user-attachments/assets/424f21ba-6d06-4bc7-846a-712330001b03)

Once the VM was turned one, navigating to the IP address I was presented with the main page:

![Pasted image 20250117153005](https://github.com/user-attachments/assets/251d6906-ab82-414d-95b1-ef41a9b91d9c)

I will be using DVWA (Damn Vulnerable Web App) below is the login information:

Username: admin
Password: password

Once logged in, I chose SQL Injection:

![Pasted image 20250117153343](https://github.com/user-attachments/assets/424e6ec3-6ff1-4c98-a8cd-24fe97e457a6)

## Burp Suite

![Pasted image 20250126074522](https://github.com/user-attachments/assets/0807da3e-1919-4d4b-a981-d3f68d0be018)

Next, I will be using Burp Suite. Burp Suite provides a variety of tools that help identify vulnerabilities in web applications. I will be using the proxy to intercept and analyze requests and responses between the attacking host and the target VM.

![Pasted image 20250117154018](https://github.com/user-attachments/assets/884672e7-320a-47c5-ad74-27a6c4ebd643)

I used the default settings for the configuration:

![Pasted image 20250117154050](https://github.com/user-attachments/assets/b608cec6-2cce-416e-8f2d-ac6688987794)

I then turned the proxy on and verified the proxy network settings:

![Pasted image 20250117095604](https://github.com/user-attachments/assets/1bf19f6f-7bfd-47ba-a31b-618c95910c2c)

I also needed to configure the proxy settings on the browser, I used Firefox:

![Pasted image 20250116152643](https://github.com/user-attachments/assets/36bfd950-e884-43e9-8d51-75a494907227)

Once that was set, I went back to DVWA and typed a number in the User ID (any number can be used):

![Pasted image 20250117095828](https://github.com/user-attachments/assets/a66b1575-dd1f-4054-b2f0-23c43e8958b4)

I then went to Burp Suite and clicked on forward and looked for the captured packet. Cookies can be found like the example below:

```
 --cookie="security=low; PHPSESSID=a8865a2cafe274fad89dd8603152787f"
```

Now that everything was set, I copied the full URL provided:

```
http://192.168.92.128/dvwa/vulnerabilities/sqli/?id=54545&Submit=Submit#
```

![Pasted image 20250117095955](https://github.com/user-attachments/assets/7f4c7b07-412c-4d02-95eb-448238e88710)

## SQLMap 

![Pasted image 20250124120556](https://github.com/user-attachments/assets/43d39e6b-5d20-456a-8001-b748cb9a0fc1)

SQLMap is an open-source tool for detecting and exploiting SQL injection vulnerabilities. SQLMap includes a powerful detection engine, lots of features, and a wide range of switches from database fingerprinting to data extraction from databases. It also allows for the execution of commands on the operating system via out-of-band connections.

I used the command below to see some of the functions and option that SQLMap has:

```
sqlmap --help | head -n 30
```
![Pasted image 20250117101836](https://github.com/user-attachments/assets/ff56483d-008b-4584-ad8e-28f45a6bab89)

Here is another look at the options that it includes for Enumeration:

![Pasted image 20250117102044](https://github.com/user-attachments/assets/5f370973-2d19-4e7c-8ba7-6c42e55f18c8)

To start, I looked at the list the available databases using the `-- dbs` parameter:

```
sudo sqlmap -u  "http://192.168.92.128/dvwa/vulnerabilities/sqli/?id=54545&Submit=Submit#" --cookie="security=low; PHPSESSID=a8865a2cafe274fad89dd8603152787f" --dbs

```

![Pasted image 20250117102701](https://github.com/user-attachments/assets/9cf675f1-b379-4ae6-92da-852e8b8c76d3)

Based on the output above, it already was able to identify that the target might be vulnerable to both SQL injection and CSS attacks. Below is the summary of information it was able to find once it completed: 

![Pasted image 20250117102616](https://github.com/user-attachments/assets/a672848d-ce29-4700-a7fe-fdf93f28bced)

Next, I searched for tables with the `--tables`  parameter:

```
sudo sqlmap -u  "http://192.168.92.128/dvwa/vulnerabilities/sqli/?id=54545&Submit=Submit#" --cookie="security=low; PHPSESSID=a8865a2cafe274fad89dd8603152787f" --tables

```

![Pasted image 20250117102932](https://github.com/user-attachments/assets/a4fc63bc-477f-492d-8203-133f140dc116)

Based on the output, it was able to find many tables. Next, I dumped the data/information using `--dump` parameter:

```
sudo sqlmap -u  "http://192.168.92.128/dvwa/vulnerabilities/sqli/?id=54545&Submit=Submit#" --cookie="security=low; PHPSESSID=a8865a2cafe274fad89dd8603152787f" --dump
```

![Pasted image 20250117103227](https://github.com/user-attachments/assets/a09243fc-c118-4308-a02c-013760c05509)

## Endpoint investigation:

Below is a list to show some examples of characters that can be seen with SQL injection attacks:

![Pasted image 20250115151846](https://github.com/user-attachments/assets/30383e18-1522-40df-bf1e-2ff3526810a4)

To start the investigation, I downloaded the Apache access. log from  the compromised endpoint:

```
scp -oHostKeyAlgorithms=+ssh-rsa  msfadmin@192.168.92.128:/var/log/apache2/access.log . 

```

![Pasted image 20250116152055](https://github.com/user-attachments/assets/dc3a3c76-8884-432e-8164-ac7f885978a1)

Reading the access log and filtering the information:

```
cat access.log | grep -iE "select|union|null" | awk '{print $1}' | sort | uniq -c
```

Here I can see that there are 129 entries all coming from the same IP address:

![Pasted image 20250117104453](https://github.com/user-attachments/assets/67ffb276-9fda-454a-bb92-55c484fc74c5)

Here is another view of the access log:

```
cat access.log | grep -iE "select|union|null" | awk '{print $1}' | head
```

![Pasted image 20250117104726](https://github.com/user-attachments/assets/47172475-2f2a-427b-8a3d-099dff2777c6)

Based on the output, I can see some of the common characters as mentioned above that are common with SQL injection attacks. Next, I used the following command in order to filter out the URL and send it to a txt file:

```
cat access.log | grep -iE "select|unionn|null" | awk '{print $7}' > url_out.txt
```

![Pasted image 20250117104857](https://github.com/user-attachments/assets/fe3d9f50-8332-498e-a776-bed480f0b13a)

I then opened the txt file created using mousepad and copied all the URLs:

![Pasted image 20250117105824](https://github.com/user-attachments/assets/271a179e-8ed7-4c2c-ae3f-295182b86325)

To decode the URLs, I used `urldecoder`:
https://www.urldecoder.io/

![Pasted image 20250117105434](https://github.com/user-attachments/assets/c689a3dc-ead0-4801-917c-3d541eaf926f)

I then copied the decoded URLs and pasted them in Sublime for easier readability:

![Pasted image 20250117105602](https://github.com/user-attachments/assets/5f24cef6-e44d-454c-9025-34b8fd5393ed)

## Network traffic analysis:

![Pasted image 20250124120344](https://github.com/user-attachments/assets/f29dea6b-350c-48f5-a7f2-a68341283e72)

I always like to have Wireshark running in the background during projects. Here, I took a packet capture during the SQL injection to see how the traffic looks and better understand the behavior of the attack.

Flow analysis:

```
tshark -nr SQLi_lm3nitro_lab.pcap -z conv,tcp -q -n | head
```

![Pasted image 20250117120705](https://github.com/user-attachments/assets/eb30953a-b30e-495e-8e4d-f2dd6646dad8)

Application analysis:

```
tshark -nr SQLi_lm3nitro_lab.pcap -Y 'http.request.full_uri' | grep -iE "select|union" | head
```

![Pasted image 20250117120519](https://github.com/user-attachments/assets/9c71f213-dd11-4d01-a919-fef902bd83c0)

## Snort 

Now that I was able to successfully perform an SQL injection attack on the target host, I updated the Snort rules to be able to detect such traffic. 

![Pasted image 20250124120443](https://github.com/user-attachments/assets/09e3e0c0-d4cb-4c47-9e94-5a14dc18d9ff)

This is the same screenshot used above and what I will use to create the Snort rules:

![Pasted image 20250115151846](https://github.com/user-attachments/assets/ef6a3d8e-0d8c-4089-b213-a30460b43cc4)

Below is a screenshot to show the traffic that was captured during the attack:

![Pasted image 20250117164809](https://github.com/user-attachments/assets/d64010de-b118-4eae-a28a-5cbeaff6c95d)

Based on the output above, I can also see that the network traffic also shows some of the characters commonly seen with SQL injection attacks. 

Below are the rules I created to detect this type of activity:

```
# Detect SQL injection attempts with ' OR '
alert tcp any any -> any 80 (msg: "SQL Injection Detected: OR pattern"; content: "' OR "; nocase; http_uri;  flow:to_server, established; priority:1; sid:10000001;)

# Detect SQL injection attempts with ' AND '
alert tcp any any -> any 80 (msg: "SQL Injection Detected: AND pattern"; content: "' AND "; nocase; http_uri;  flow:to_server, established; priority:1; sid:10000002;)

# Detect SQL injection attempts with ' UNION '
alert tcp any any -> any 80 (msg: "SQL Injection Detected: UNION pattern"; content: "UNION"; nocase; http_uri; flow:to_server, established; priority:1;sid:10000003;)

# Detect SQL injection attempts with '--'
alert tcp any any -> any 80 (msg: "SQL Injection Detected: Commenting out"; content: "--"; nocase; http_uri; flow:to_server, established; priority:1; sid:10000004;)

# Detect SQL injection attempts with single quote
alert tcp any any -> any 80 (msg: "SQL Injection Detected: Single Quote"; content: "'"; nocase; http_uri; flow:to_server, established; priority:1; sid:10000005;)

# Detect error-based SQL injection with '%27'
alert tcp any any -> any 80 (msg: "SQL Injection Detected: Error-based payload"; content: "%27"; http_uri; flow:to_server, established; priority:1; sid:10000006;)
```

I made sure to add the rules to the main location: `/etc/snort/rules/local.rules`

![Pasted image 20250117165422](https://github.com/user-attachments/assets/65a169c6-a2c4-477d-a435-d326df98bdd0)

Now that I had the rules in place, I tested them against the PCAP previously captured:
```
sudo snort -c /etc/snort/snort.conf -r SQLi_lm3nitro_lab.pcap -A console -k none -q
```

![Pasted image 20250117164216](https://github.com/user-attachments/assets/389912f2-37e7-4851-93e2-8ce8e686c179)

Two alerts were generated. Below is a look into one of the alerts:
```
sudo tcpdump -nr snort.log.1737150135 -A | head 
```

![Pasted image 20250117164958](https://github.com/user-attachments/assets/004dfbcb-aab1-484d-89b7-c5bb56e25a17)

## Conclusion

I was able to successfully set up a Metasploitable 2 (Linux) VM and used SQLMap to perform a SQL injection attack against it while capturing the traffic. This project allowed me to better understand how SQL queries interact with databases and how it behaves on the network.

In the traffic captured, I was able to observe the malicious SQL queries and URL parameters. By analyzing the patterns of suspicious queries and payloads, I was able to craft custom rules that look for common injection techniques and unusual request behaviors to ensure that any malicious SQL injection attempts are flagged in real-time.
