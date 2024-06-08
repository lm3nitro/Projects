
![Pasted image 20240606094506](https://github.com/lm3nitro/Projects/assets/55665256/d218006c-7a0c-4477-8cd6-6bd6bf48a476)


Squid is a caching and forwarding HTTP web proxy. It has a wide variety of uses, including speeding up a web server by caching repeated requests, caching World Wide Web (WWW), Domain Name System (DNS), and other network lookups for a group of people sharing network resources, and aiding security by filtering traffic.

Note:

I'm going to be installing a feature of  squid called "Squid-in-the-middle" for  decryption and encryption of straight CONNECT and transparently redirected SSL traffic, using configurable self-signed CA certificate.

System information:

Ubuntu 22.04.4 LTS x86_64
Squid Cache: Version 5.9
OpenSSL 3.0.2 15


![Pasted image 20240606154403](https://github.com/lm3nitro/Projects/assets/55665256/ec7d8d2a-0f52-4b57-90c8-4d449bb18adc)


Network diagram:

![Pasted image 20240607185534](https://github.com/lm3nitro/Projects/assets/55665256/a3cde0e0-8eac-4f5d-a8b3-9411054d3796)


# Downloading Squid Proxy:


Main website:


![Pasted image 20240605210938](https://github.com/lm3nitro/Projects/assets/55665256/6cb214c0-b4c0-447d-991d-396b56370a3c)


Looking for version 5.9

![Pasted image 20240605211601](https://github.com/lm3nitro/Projects/assets/55665256/d7998bbf-acaf-4a2b-bdd7-0997719267a8)



Note:  On the ubuntu server create a directory where we're going to work with the squid files.


cd /usr/local/src/

![Pasted image 20240605210812](https://github.com/lm3nitro/Projects/assets/55665256/b62718b9-8b1f-440c-a279-b011f762f486)


Downloading the code:

wget https://www.squid-cache.org/Versions/v5/squid-5.9.tar.gz

![Pasted image 20240605211743](https://github.com/lm3nitro/Projects/assets/55665256/72ebf5d7-3e51-402f-8fef-97aa411356e4)


# Decompress the file:


tar zxvf squid-5.9.tar.gz

# Installing dependencies:



apt install build-essential openssl libssl-dev pkg-config


![Pasted image 20240605212332](https://github.com/lm3nitro/Projects/assets/55665256/8dbd13c8-ecbb-42fa-af0b-2c1a2df93cb1)



./configure --with-default-user=proxy --with-openssl --enable-ssl-crtd

![Pasted image 20240605212822](https://github.com/lm3nitro/Projects/assets/55665256/e3b9230b-993e-4021-bcca-0605b60f75c6)

Once it finishes type:


make

![Pasted image 20240605213219](https://github.com/lm3nitro/Projects/assets/55665256/5e401121-e39a-4d43-91d8-7fcd183ceba3)


#### Edit the file /etc/ssl/openssl.cnf

add the following line under [ v3_ca ]

keyUsage =  cRLSign, keyCertSign

![Pasted image 20240605215335](https://github.com/lm3nitro/Projects/assets/55665256/c401b37e-723c-4b71-b5d6-dda140dac593)

![Pasted image 20240605215314](https://github.com/lm3nitro/Projects/assets/55665256/30e52bda-ebad-4576-8456-0565d21e7446)



# Generating the certificates:

First create directory under /tmp/ssl_cert

cd /tmp/ssl_cert

![Pasted image 20240605215537](https://github.com/lm3nitro/Projects/assets/55665256/cb332fcd-673c-4603-abc5-df458ef58fdb)



# Working with certificate:


Creating all necessary certificates so squid can decrypt the traffic.


openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -extensions v3_ca -keyout squid-self-signed.key -out squid-self-signed.crt


![Pasted image 20240605220029](https://github.com/lm3nitro/Projects/assets/55665256/ed051ca4-b883-4237-8362-9e040928c49c)




openssl x509 -in squid-self-signed.crt -outform DER -out squid-self-signed.der


![Pasted image 20240605220308](https://github.com/lm3nitro/Projects/assets/55665256/6ad6db04-a677-4eac-b208-c542b632d869)



openssl x509 -in squid-self-signed.crt -outform PEM -out squid-self-signed.pem

![Pasted image 20240605220436](https://github.com/lm3nitro/Projects/assets/55665256/df8b0dbe-a33e-4cec-a85f-f3e053a74be0)


openssl dhparam -outform PEM -out squid-self-signed_dhparam.pem 2048

![Pasted image 20240605220645](https://github.com/lm3nitro/Projects/assets/55665256/db76f400-3519-4d75-a083-453cb5a1d928)

![Pasted image 20240605220738](https://github.com/lm3nitro/Projects/assets/55665256/999a6752-5d47-4a65-af4e-6eb00419f37a)


# Finishing the install of the squid proxy:


Once all the certificates has been create, it's now time for finishing the installation of the squid. Going back to  /usr/local/src/squid-5.9/

cd /usr/local/src/squid-5.9/

then type

make install



![Pasted image 20240605221347](https://github.com/lm3nitro/Projects/assets/55665256/889acb4a-4a10-4374-8956-017aee583ba5)




### Let's go back to /tmp/ssl_cert:


Move: Moving the certificates to squid directory. 

cp -rf /tmp/ssl_cert /usr/local/squid/etc/ssl_cert

![Pasted image 20240605221633](https://github.com/lm3nitro/Projects/assets/55665256/6605e950-9fcc-4d15-a714-fc2f7f0a990b)



cp /usr/local/squid/etc/ssl_cert/squid-self-signed.pem /usr/local/share/ca-certificates/squid-self-signed.crt


![Pasted image 20240605222107](https://github.com/lm3nitro/Projects/assets/55665256/f5e2095f-b4cf-4ea0-915c-10c464eb597d)



# Updating the CA certificates:

update-ca-certificates

![Pasted image 20240605222300](https://github.com/lm3nitro/Projects/assets/55665256/005a6e98-e7fc-490d-b675-52251eb70e89)


# Edit Squid.conf file 
````
Adding the follwing lines:


acl intermediate_fetching transaction_initiator certificate-fetching
http_access allow intermediate_fetching

acl all src all
http_access allow all


http_port 3128 tcpkeepalive=60,30,3 ssl-bump generate-host-certificates=on dynamic_cert_mem_cache_size=20MB tls-cert=/usr/local/squid/etc/ssl_cert/squid-self-signed.crt tls-key=/usr/local/squid/etc/ssl_cert/squid-self-signed.key cipher=HIGH:MEDIUM:!LOW:!RC4:!SEED:!IDEA:!3DES:!MD5:!EXP:!PSK:!DSS options=NO_TLSv1,NO_SSLv3,NO_SSLv2,SINGLE_DH_USE,SINGLE_ECDH_USE tls-dh=prime256v1:/usr/local/squid/etc/ssl_cert/squid-self-signed_dhparam.pem
sslcrtd_program /usr/local/squid/libexec/security_file_certgen -s /usr/local/squid/var/logs/ssl_db -M 20MB
sslcrtd_children 5
ssl_bump server-first all
ssl_bump stare all
sslproxy_cert_error deny all



maximum_object_size 6 GB
cache_mem 8192 MB
cache_dir ufs /usr/local/squid/var/cache/squid 32000 16 256


coredump_dir /usr/local/squid/var/cache/squid

cache allow all

````
### then  save the file and apply permissions :


chown -R proxy:proxy /usr/local/squid


![Pasted image 20240605231350](https://github.com/lm3nitro/Projects/assets/55665256/df7d0500-a626-4041-8acd-e393cdc95290)


sudo -u proxy -- /usr/local/squid/libexec/security_file_certgen -c -s /usr/local/squid/var/logs/ssl_db -M 20MB


![Pasted image 20240605231651](https://github.com/lm3nitro/Projects/assets/55665256/02a30a54-d9f2-422b-a76d-6201152ba7d4)


## Note: locating the squid binary in my case is under:


/usr/local/squid/sbin/squid


![Pasted image 20240605233247](https://github.com/lm3nitro/Projects/assets/55665256/09425b3c-b2a7-4444-b757-3c940748e2e1)



All squid files that we ever need will be under /usr/local/squid/

![Pasted image 20240606162127](https://github.com/lm3nitro/Projects/assets/55665256/b9e885c2-b408-4f3f-8c2d-2b7169dc7b9d)



# Starting Squid:


sudo -u proxy -- /usr/local/squid/sbin/squid -z 

sudo -u proxy -- /usr/local/squid/sbin/squid -d 10


![Pasted image 20240605233617](https://github.com/lm3nitro/Projects/assets/55665256/602d96cd-7af8-47fc-8201-06c4620d83a2)


# Check squid processes status:


![Pasted image 20240605235057](https://github.com/lm3nitro/Projects/assets/55665256/446c3662-9129-442d-ab61-f6c9b8e65e58)



# Installing colours to logs: (Optional)


![Pasted image 20240605233802](https://github.com/lm3nitro/Projects/assets/55665256/d40b8869-c438-4059-a9b1-854ab932bf36)


### Verify cache logs use:


tail -f /usr/local/squid/var/logs/cache.log

![Pasted image 20240605234002](https://github.com/lm3nitro/Projects/assets/55665256/506e7742-47fb-48c7-b9cd-06623e685934)




### Verify access log use:

Note: There is not log, I haven't configure the proxy information on the Windows client.

tail -f /usr/local/squid/var/logs/access.log


# Installing calamaris: (optional)

![Pasted image 20240605234421](https://github.com/lm3nitro/Projects/assets/55665256/e852737c-081b-4455-b808-da58410847be)

![Pasted image 20240605234408](https://github.com/lm3nitro/Projects/assets/55665256/84950571-000c-4427-820d-fbb5c9f3332f)


#### Verify squid is listening:


ss -tlp


![Pasted image 20240605234757](https://github.com/lm3nitro/Projects/assets/55665256/560d7752-6409-4ec5-84c6-51b8a9cb4549)



 ps -aux | grep 109564

![Pasted image 20240605235300](https://github.com/lm3nitro/Projects/assets/55665256/de0931ba-97a3-4503-9527-842a441937bc)


# Downloading certificates to Window client:



![Pasted image 20240605235638](https://github.com/lm3nitro/Projects/assets/55665256/84bddefe-778a-41fb-b646-6deb7cd50612)


![Pasted image 20240606000152](https://github.com/lm3nitro/Projects/assets/55665256/896346b5-200b-4f45-8942-d524aece9cd8)



### Verify the IP of the squid proxy and getting into the Proxy Windows system settings:


![Pasted image 20240606000533](https://github.com/lm3nitro/Projects/assets/55665256/f2c9068c-6855-45ae-85f1-d53e1c59053e)


![Pasted image 20240606000605](https://github.com/lm3nitro/Projects/assets/55665256/c100b96a-08f2-4e1e-bcc8-d78917462f1e)



### Verify connection status at the windows client:

![Pasted image 20240606002736](https://github.com/lm3nitro/Projects/assets/55665256/d58af6d2-984f-4b28-8e54-3dbe2e8661fd)


### Verify the logs at the proxy:


![Pasted image 20240606002237](https://github.com/lm3nitro/Projects/assets/55665256/99ac4719-6163-48fb-b2e6-904e6bee1fd4)


### Check the traffic at the Windows client :

Let;s go to lichess.org for testing



![Pasted image 20240606004834](https://github.com/lm3nitro/Projects/assets/55665256/3d20665e-a764-410f-9df1-363f38f543ce)




### Verify the certificate:


![Pasted image 20240606010804](https://github.com/lm3nitro/Projects/assets/55665256/fe40ddec-5939-47e4-a4fa-2c7fb9de8cd7)


### Windows client connecting to the Squid proxy:


![Pasted image 20240606003952](https://github.com/lm3nitro/Projects/assets/55665256/f5fe00e1-c607-46e4-83f9-ec7b35654d73)


### Decrypted Lichess.org traffic at the proxy:

![Pasted image 20240606004112](https://github.com/lm3nitro/Projects/assets/55665256/dbed89ea-0ca6-45b6-838b-1f6a4c8497cb)


# Decrypting traffic lichess.org traffic at the Windows Client using Wireshark:



![Pasted image 20240606003503](https://github.com/lm3nitro/Projects/assets/55665256/04375c5b-a2e6-495c-a99c-09e87571bd0f)


 After the traffic has been decrypted, It seems like lichess.org is using HTTP/1.1.
 


![Pasted image 20240606003915](https://github.com/lm3nitro/Projects/assets/55665256/f3b4bfe5-f2cf-43e2-a70a-0f747b932b90)


It matches:

![Pasted image 20240606004627](https://github.com/lm3nitro/Projects/assets/55665256/0db23a17-b0a5-4d19-a22e-bd2cf1b35de1)


### Exporting HTTP objects from lichess.org traffic for further analysis :



==============20240606005510



### Exported objects:


![[Attachments/Pasted image 20240606005706.png]]


# Downloading Firefox:

Traffic logs:

We can see the installer.exe

![[Attachments/Pasted image 20240606011922.png]]

Proxy logs:

We can see the installer.exe at the proxy:

![[Attachments/Pasted image 20240606011518.png]]

Logs at the SIEM:

All the squid traffic have been sent to Splunk. We can see the installer.exe. 


![[Attachments/Pasted image 20240606093227.png]]




# Verify the Cache Squid is working:


In the squid.conf under /usr/local/squid/etc/ I create a cache rule to cache any of the files listed  there 

![[Attachments/Pasted image 20240607174629.png]]


The squid cache is under /usr/local/squid/var/cache/

![[Attachments/Pasted image 20240607172049.png]]


searching all the directories and files search recursively:



find . -type f | xargs grep -E  installer


![[Attachments/Pasted image 20240607172542.png]]

Another way to search:


find .  -type f -print0 | xargs -0 grep -l "installer"


![[Attachments/Pasted image 20240607172414.png]]

Opening the last file  with nano ./squid/00/0E/00000EE4

![[Attachments/Pasted image 20240607172837.png]]



Opening another file on the list nano squid/00/09/0000097A

We got the actual installer.exe file binary!

![[Attachments/Pasted image 20240607173631.png]]



xxd squid/00/09/0000097A | grep -iE  'installer|exe|dos'

![[Attachments/Pasted image 20240607180003.png]]

### Logs colorized with CCZE:


cat  /usr/local/squid/var/logs/access.log | ccze -A -C -o noscroll | grep -iE "installer.exe"

![[Attachments/Pasted image 20240606094234.png]]



## Disabling proxy in system settings in Windows 10 client :

![[Attachments/Pasted image 20240606100136.png]]



Note the WinAPI will allow traffic to the whole operating system "proxy capable apps " to the proxy:

As you can on the logs, we are getting traffic going to Microsoft even though I didn't browse Microsoft website:

![[Attachments/Pasted image 20240606110825.png]]


In order to fix that we will need install a Browser that doesn't use the WinAPI, and block all the traffic at the firewall level from this Win host. Firefox in this case a separate API for the application and does not use WinAPI



# Configuring proxy in Firefox:


![[Attachments/Pasted image 20240606095903.png]]




# Rule creation on the Fortinet firewall:


Creating a firewall rule to allow only Squid proxy traffic to internet. In this situation the proxy is already allow "ALL outbound" The only we need is to create a new entry to block the Windows Client PC on 10.10.100.101

![[Attachments/Pasted image 20240606105235.png]]


### Looking at the hit count:


![[Attachments/Pasted image 20240606104922.png]]


### Traffic that is being block a the Windows Client:

![[Attachments/Pasted image 20240606105941.png]]



# Proxy IP information:

![[Attachments/Pasted image 20240606110515.png]]
Firewall logs allowing the proxy:


![[Attachments/Pasted image 20240606110439.png]]


Now the firewall is blocking all the traffic from Clien using WinAPI with IP 10.10.100.101 going outbound that is no passing for the proxy. We're enforcing our proxy policy to use only Firefox app to the internet.


# Disable Squid Cache: (Optional)


On the configuration file located under /usr/local/squid/etc
 remove these lines:

maximum_object_size 6 GB
cache_mem 8192 MB
cache_dir ufs /usr/local/squid/var/cache/squid 32000 16 256


squid.conf 

![[Attachments/Pasted image 20240606162914.png]]

Note: You can use the cache access list to make Squid never cache any response.
#### cache deny all


# Block custom domain with squid:


Create  domain blacklist in Squid.conf

Edit the /etc/squid/squid.conf file and add the following settings at the beginning of the file: 

acl domain_blacklist dstdomain "/usr/local/squid/etc/domain_blacklist.txt"
http_access deny all domain_blacklist


![[Attachments/Pasted image 20240606170758.png]]




Create the /usr/local/squid/etcdomain_blacklist.txt file and add the domains you want to block. For example, to block access to example.com including subdomains and to block example.net, add: 

.lm3nitro.example.com
lm3nitro.squid.lab.example.com

In my case, I will be blocking lichess and chess.com

![[Attachments/Pasted image 20240606171228.png]]
Kill the squid process:

![[Attachments/Pasted image 20240606171520.png]]


Starting the squid service by typing:


sudo -u proxy -- /usr/local/squid/sbin/squid -z
sudo -u proxy -- /usr/local/squid/sbin/squid -d 10


Verify the service again, we can see that we got new pid numbers:


![[Attachments/Pasted image 20240606171819.png]]


Let's go to lichess.org and chess.com

![[Attachments/Pasted image 20240606170337.png]]

![[Attachments/Pasted image 20240606171917.png]]


### Squid has blocked the traffic:

tail  -f /usr/local/squid/var/logs/access.log | grep -E  "lichess|chess"

![[Attachments/Pasted image 20240606172048.png]]





cat /usr/local/squid/var/logs/access.log | grep -E  ' eicar|zip'

![[Attachments/Pasted image 20240607182009.png]]




# Monitor process and network traffic:

Installing nload

![[Attachments/Pasted image 20240607184651.png]]


Installing htop

![[Attachments/Pasted image 20240607213149.png]]

Installing tcptrack

![[Attachments/Pasted image 20240607213409.png]]


![[Attachments/Pasted image 20240607213757.png]]

# Securing the Squid Proxy :




![[Attachments/Pasted image 20240607211638.png]]


ClamAV is a popular open source anti-virus tool to detect malicious software or malware. While it calls itself an antivirus engine, used in a variety of situations including email scanning, web scanning, and end point security.

# Installing ClamAV

apt install clamav clamav-daemon

![[Attachments/Pasted image 20240607203552.png]]

# Verify version:

clamscan --version

![[Attachments/Pasted image 20240607203705.png]]

# Checking status

systemctl status clamav-freshclam

![[Attachments/Pasted image 20240607203900.png]]

### We need to stop to update DB:

systemctl stop clamav-freshclam

![[Attachments/Pasted image 20240607204043.png]]

### Updating DB

freshclam

![[Attachments/Pasted image 20240607204139.png]]

Start clamav now that DB was updated, use enable for persistence

systemctl enable clamav-freshclam --now

![[Attachments/Pasted image 20240607204328.png]]

Note to disable: systemctl disable clamav-freshclam --now

### Verify clamav files and directory:


ls -l /var/lib/clamav/

![[Attachments/Pasted image 20240607204513.png]]


# Tesing ClamAV:


### Downloading antivirus test files:

It probably won't encounter many viruses, as they have become rare. It is more likely to find other forms of malware like worms, backdoors, and ransomware.


![[Attachments/Pasted image 20240607210556.png]]



![[Attachments/Pasted image 20240607210724.png]]

### Scanning current directory .

clamscan .


![[Attachments/Pasted image 20240607210918.png]]





