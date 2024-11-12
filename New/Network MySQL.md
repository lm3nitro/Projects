# MySQL

MySQL is an open-source relational database management system (RDBMS) that uses a language called SQL (Structured Query Language) to perform tasks like adding, updating, and querying data. MySQL is widely used for websites and applications, is known for being fast and reliable, and works on different operating systems.

![Pasted image 20241003122935](https://github.com/user-attachments/assets/e5d42868-e3d4-42b4-9dd1-a942e1d92bf8)

Below is a view at the conversation between client and server:

![Pasted image 20241003123018](https://github.com/user-attachments/assets/ff3078a3-b693-4c73-b472-87c6c452d738)

The process works like this: MySQL creates a database to organize data in tables, clients send requests using SQL statements, and the server responds with the requested information.

### Scope:

In this exercise, I will cover how to enumerate MySQL using nmap and metaspolit. Provided the information found through the enumeration, I will then exploit MySQL and crack a users password using John the Ripper allowing me to connect directly to the database server. 

### Tools and Technology:
Linux, Nmap, Wireshark, MySQL, Metasploit, and John the Ripper

## Enumerating MySQL :
![Pasted image 20241003124855](https://github.com/user-attachments/assets/db6e2d27-aab5-4ac6-b7d6-cebb58c8d301)

Enumeration is the process of discovering and gathering information about network services, hosts, and their configurations. There are many tools that can be used to enumerate. Here I will be using nmap and Metasploit. 

First, I used nmap to see what information I could gather from the target:

```
nmap -A 10.10.137.40 -nvvv
```

![Pasted image 20241003125145](https://github.com/user-attachments/assets/b6683923-409c-46e8-b9d9-a6598d0ed836)

Based on the output, I see that port 22 (SSH) and port 3306 (MySQL) are both open. I can also see information on the OS and the MySQL version that is running on the host. 

> [!IMPORTANT]  
> Version 5.x is susceptible to a user enumeration attack due to different messages during login when using old authentication mechanism from versions 4.x and earlier.

I then used one of the scripts included with nmap to enumerate MySQL:

```
nmap -p 3306 --script=mysql-enum  10.10.137.40 -nvvv
```

![Pasted image 20241003125810](https://github.com/user-attachments/assets/f5b14bf4-8305-4db6-9fc8-f40164af24a6)

Based on the output, I was able to see the list of valid usernames. 

## Wireshark:

![Pasted image 20241003132213](https://github.com/user-attachments/assets/2da4f9b4-8a19-4c67-bbad-b6378ecd38f1)

While enumerating MySQL, I also had Wireshark running to view the network traffic generated with the scan:

![Pasted image 20241003130529](https://github.com/user-attachments/assets/3be37a48-f294-4ac0-9f68-55d8291deabe)

Wireshark also shows version: 5.7.29, Ubuntu 18.04.1. Also, taking a closer look at one of the packets, I could see information on root and the password:

![Pasted image 20241003130425](https://github.com/user-attachments/assets/bb29feff-64f0-4ca9-9b3d-4b67aeec79fa)

Another observation about the traffic generated was the many connections that never established and had a short duration:

![Pasted image 20241003131447](https://github.com/user-attachments/assets/23904ef3-0b23-46f9-ab26-48f983c21771)

Another observation was the error response 1043 as seen below:
![Pasted image 20241003131314](https://github.com/user-attachments/assets/89dc25a9-c835-42d8-b3f7-3b636c62149e)

A closer look at the error:

![Pasted image 20241003131055](https://github.com/user-attachments/assets/7f009dfa-78d7-4192-a257-22dcc20601a8)

> [!IMPORTANT]  
> There are common errors related to MySQL:
> + Error 1043: Often related to using the wrong protocol or an incompatible version.
> + Error 1045: Typically, due to incorrect username or password.
> + Error 2002: Indicates that the MySQL server is not responding or cannot be found.
> + Error 1146: Usually means that the specified table does not exist in the database.

<!-- This is commented out.
## MySQL Client:

![Pasted image 20241003132047](https://github.com/user-attachments/assets/4fd85a84-f4dd-48c9-9078-02ea74e3be64)

Another tool to use is the MySQL client. Although I did not use it, this is the command needed to install MySQL client:

```
sudo apt install default-mysql-client
```

![Pasted image 20241003130002](https://github.com/user-attachments/assets/e164d2c0-88fa-407a-9486-bece70f5b3cb)

The following command can be used to specify the hostname of the MySQL server you wish to connect to:

```
mysql -h 10.10.137.40 -u root -p "password"
```
 -->
## Metasploit:

![Pasted image 20241003132013](https://github.com/user-attachments/assets/146172de-0367-4580-a533-618019a14fb9)

Metasploit contains a variety of modules that can be used to enumerate MySQL databases, making it easy to gather valuable information. To start, I will be using the the mysql_version module, as its name implies, it scans a host or range of hosts to determine the version of MySQL that is running. I have previously done this using Nmap but wanted to cover different tools that can be used. 

```
use auxiliary/scanner/mysql/mysql_version
show options
set RHOSTS 192.168.1.200-254
set THREADS 20
run
```

I also used the Scanner MySQL Auxiliary Module:

![Pasted image 20241003140152](https://github.com/user-attachments/assets/a29999ec-2daa-4f7a-b7aa-aee0750a4646)

```
use auxiliary/scanner/mysql/mysql_login
show options
set PASSWORD password
set RHOSTS 10.10.137.40
set USERNAME root
run

```

The ouput:

![Pasted image 20241003140121](https://github.com/user-attachments/assets/7ad9f83d-7766-457a-809c-cd5b54d7e815)

The same results as Nmap, it was also able to identify the MySQL version, in this case version 5.7.29.

While performing this enumeration, I also had Wireshark running in the back end, and I was able to capture the same information as previously captured:

5.7.29-0ubuntu0.18.04.1

![Pasted image 20241003135235](https://github.com/user-attachments/assets/f401a1bc-f174-4086-87ba-182a08fb0ffa)

A closer look at one of the packets:

![Pasted image 20241003135217](https://github.com/user-attachments/assets/7e9a1046-8cdc-415c-99a3-68450304fc79)

Next, I used the `show databases` command. This will list all of the available databases:

```
set SQL show databases
run
```

![Pasted image 20241003140442](https://github.com/user-attachments/assets/aac492e5-ab4b-464c-8312-ff9c76e02308)

A view of the traffic in Wireshark:

![Pasted image 20241003140525](https://github.com/user-attachments/assets/c37587b3-c779-4405-986d-4e52d198434a)

## Exploiting MySQL:

At this point, I was able to gain more sensitive information than just database names. I now have the following information on the MySql server:

+ MySQL server credentials
+ The running version of MySQL
+ The number of Databases, and their names.

Given the information I know, I then loaded mysql_hashdump. The mysql_hashdump will gather any additional password hashes:

```
auxiliary/scanner/mysql/mysql_hashdump 
```

After loading the module, you are presented with options:

![Pasted image 20241003143211](https://github.com/user-attachments/assets/e0c232ad-5e27-45a8-8d1f-5a65e27c3d07)

I entered the following:

```
set PASSWORD password
set USERNAME  root
set RHOTS 10.10.137.40
```

By providing the information above, below is the list of hashes that it was able to provide. Here I can see the user Carl:

![Pasted image 20241003144109](https://github.com/user-attachments/assets/b3e71609-32e2-4725-8e8d-4f0c34a87bc1)

## John the Ripper:

![Pasted image 20241003145245](https://github.com/user-attachments/assets/b36c1cdc-2c3c-4577-8133-3451a333cda9)

John the Ripper is a popular open-source password cracking software tool. It's primarily used for learning about password security and testing the strength of passwords. In this case, I will be using it to get the password using the hash that i previously found:

1. First, I copied all the hashes to a document. I named the file `sql_hasdump.txt`:

![Pasted image 20241003143819](https://github.com/user-attachments/assets/21b5a4ff-fe4f-43a4-904e-f456e21e04bd)

I then used the following command:

```
john sql_hasdump.txt
```

Here I was provided with the password for the user `Carl`:

![Pasted image 20241003144501](https://github.com/user-attachments/assets/351dc81e-9087-4306-b3db-88ef14a57c8a)

Now that I was able to find the password, I then tested the credentials via SSH knowing that the SSH was identified as being opened earlier:

```
ssh carl@10.10.137.40
```

After using SSH and entering the credentials, I was able to authenticate as the user Carl. 
![Pasted image 20241003144429](https://github.com/user-attachments/assets/920c8bd5-4e75-4d64-88cd-ca77503f2d9b)

### Summary:

In conclusion, I was able to enumerate the MySQL server using both **nmap** and **metasploit** which provided me with a series of information such as the version running, ports that were opened, list of databases, and a list of password hashes. Using this information, I was then able to exploit the MySQL server by copying the hashes and using **John the Ripper** to find the password for the user Carl. Once I had the password, I was then able to SSH into the server as the user. 

Overall, enumeration is critical because it allows you to gather detailed information about a target system, network, or application. Some key benefits are:

+ Identifying Vulnerabilities
+ Understanding Attack Surface
+ Mapping the Environment
+ User and Service Discovery

