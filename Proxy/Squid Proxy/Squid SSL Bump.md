# Squid SSL Bump

![Pasted image 20240606094506](https://github.com/lm3nitro/Projects/assets/55665256/d218006c-7a0c-4477-8cd6-6bd6bf48a476)

Squid is a caching and forwarding HTTP web proxy. Squid SSL Bump is a feature in Squid Proxy, that allows it to intercept and decrypt SSL/TLS traffic in order to inspect, log, or manipulate the data. This feature enables the inspection of encrypted traffic, providing the ability to filter, log, and analyze HTTPS communications

### Scope: 

I will be installing and configuring Squid Proxy with SSL Bump to intercept and decrypt HTTPS traffic for content filtering and inspection. Once Squid is set up, I will test its functionality by routing traffic from a Windows 10 VM to verify SSL bumping and domain blocking features. I will also be hardening the Squid proxy server by installing ClamAV. 

### Tools and Technology:
Ubuntu, Squid Proxy, OpenSSL, Windows 10, Wireshark, Splunk, Fortinet, and ClamAV

Network diagram:

![Pasted image 20240607185534](https://github.com/lm3nitro/Projects/assets/55665256/a3cde0e0-8eac-4f5d-a8b3-9411054d3796)

Note:
I'm going to be installing a feature of squid called "Squid-in-the-middle" for  decryption and encryption of straight CONNECT and transparently redirected SSL traffic, using configurable self-signed CA certificate.

## Installing Squid Proxy

To start, I will be installing Squid Proxy on my Ubuntu VM. Below is the System information:

+ Ubuntu 22.04.4 LTS x86_64
+ Squid Cache: Version 5.9
+ OpenSSL 3.0.2 15

![Pasted image 20240606154403](https://github.com/lm3nitro/Projects/assets/55665256/ec7d8d2a-0f52-4b57-90c8-4d449bb18adc)

First, I need to download Squid Proxy. I went to the main website:

![Pasted image 20240605210938](https://github.com/lm3nitro/Projects/assets/55665256/6cb214c0-b4c0-447d-991d-396b56370a3c)

At the time of this writing, version 5.9 is the latest:

![Pasted image 20240605211601](https://github.com/lm3nitro/Projects/assets/55665256/d7998bbf-acaf-4a2b-bdd7-0997719267a8)

> [!NOTE]  
> On the Ubuntu server, I created a directory where the squid files I will be working on will be placed. 

```
cd /usr/local/src/
```

![Pasted image 20240605210812](https://github.com/lm3nitro/Projects/assets/55665256/b62718b9-8b1f-440c-a279-b011f762f486)

After navigating to the directory where I wanted Squid to downloaded, I used `wget` to download it:

```
wget https://www.squid-cache.org/Versions/v5/squid-5.9.tar.gz
```

![Pasted image 20240605211743](https://github.com/lm3nitro/Projects/assets/55665256/72ebf5d7-3e51-402f-8fef-97aa411356e4)

Next, I extracted the files:

```
tar zxvf squid-5.9.tar.gz
```

## Installing Dependencies:

After downloading Squid Proxy, I then began to install the needed dependencies:

```
apt install build-essential openssl libssl-dev pkg-config
```

![Pasted image 20240605212332](https://github.com/lm3nitro/Projects/assets/55665256/8dbd13c8-ecbb-42fa-af0b-2c1a2df93cb1)

Configuring Squid Proxy:

```
./configure --with-default-user=proxy --with-openssl --enable-ssl-crtd
```

![Pasted image 20240605212822](https://github.com/lm3nitro/Projects/assets/55665256/e3b9230b-993e-4021-bcca-0605b60f75c6)

Once it completed, I used the following command to build it:

```
make
```

![Pasted image 20240605213219](https://github.com/lm3nitro/Projects/assets/55665256/5e401121-e39a-4d43-91d8-7fcd183ceba3)

## Configuration

After, I need to make some changes to the openssl configuration, I edited the following file `/etc/ssl/openssl.conf`

![Pasted image 20240605215335](https://github.com/lm3nitro/Projects/assets/55665256/c401b37e-723c-4b71-b5d6-dda140dac593)

I added the following line under [ v3_ca ]

```
keyUsage =  cRLSign, keyCertSign
```

![Pasted image 20240605215314](https://github.com/lm3nitro/Projects/assets/55665256/30e52bda-ebad-4576-8456-0565d21e7446)

Next, I generated the needed certificates. To do this, I created a directory under `/tmp/ssl_cert`

```
cd /tmp/ssl_cert
```

![Pasted image 20240605215537](https://github.com/lm3nitro/Projects/assets/55665256/cb332fcd-673c-4603-abc5-df458ef58fdb)

I then created all necessary certificates that the Squid Proxy will use to decrypt the traffic.

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -extensions v3_ca -keyout squid-self-signed.key -out squid-self-signed.crt
```

![Pasted image 20240605220029](https://github.com/lm3nitro/Projects/assets/55665256/ed051ca4-b883-4237-8362-9e040928c49c)

```
openssl x509 -in squid-self-signed.crt -outform DER -out squid-self-signed.der
```

![Pasted image 20240605220308](https://github.com/lm3nitro/Projects/assets/55665256/6ad6db04-a677-4eac-b208-c542b632d869)

```
openssl x509 -in squid-self-signed.crt -outform PEM -out squid-self-signed.pem
```

![Pasted image 20240605220436](https://github.com/lm3nitro/Projects/assets/55665256/df8b0dbe-a33e-4cec-a85f-f3e053a74be0)

```
openssl dhparam -outform PEM -out squid-self-signed_dhparam.pem 2048
```

![Pasted image 20240605220645](https://github.com/lm3nitro/Projects/assets/55665256/db76f400-3519-4d75-a083-453cb5a1d928)

All the completed certificates:

![Pasted image 20240605220738](https://github.com/lm3nitro/Projects/assets/55665256/999a6752-5d47-4a65-af4e-6eb00419f37a)

## Finishing Squid Proxy Install:

Once all the certificates had been created, I finished the installation of the Squid Proxy. I first needed to navigate back to the directory where it was initially downloaded to: `/usr/local/src/squid-5.9/`

```
cd /usr/local/src/squid-5.9/
```

Typed the following to complete the installation/build:

```
make install
```

![Pasted image 20240605221347](https://github.com/lm3nitro/Projects/assets/55665256/889acb4a-4a10-4374-8956-017aee583ba5)

Once that completed, I went back to the certificate's directory: `/tmp/ssl_cert`. I then copied the certificates previously created to the Squid Proxy directory:

```
cp -rf /tmp/ssl_cert /usr/local/squid/etc/ssl_cert
```

![Pasted image 20240605221633](https://github.com/lm3nitro/Projects/assets/55665256/6605e950-9fcc-4d15-a714-fc2f7f0a990b)

```
cp /usr/local/squid/etc/ssl_cert/squid-self-signed.pem /usr/local/share/ca-certificates/squid-self-signed.crt
```

![Pasted image 20240605222107](https://github.com/lm3nitro/Projects/assets/55665256/f5e2095f-b4cf-4ea0-915c-10c464eb597d)

I then updated the CA certificates:

```
update-ca-certificates
```

![Pasted image 20240605222300](https://github.com/lm3nitro/Projects/assets/55665256/005a6e98-e7fc-490d-b675-52251eb70e89)

## Squid Configuration:

In order to configure the Squid Proxy, I first edited the `Squid.conf` file. I added the following lines:

```
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

Saved the file and applied the needed permissions:

```
chown -R proxy:proxy /usr/local/squid
```

![Pasted image 20240605231350](https://github.com/lm3nitro/Projects/assets/55665256/df7d0500-a626-4041-8acd-e393cdc95290)

I then used the following command to generate the SSL certificates for Squid's SSL bumping feature:

```
sudo -u proxy -- /usr/local/squid/libexec/security_file_certgen -c -s /usr/local/squid/var/logs/ssl_db -M 20MB
```

![Pasted image 20240605231651](https://github.com/lm3nitro/Projects/assets/55665256/02a30a54-d9f2-422b-a76d-6201152ba7d4)

Next, I located Squid's binary, in my case it was under `/usr/local/squid/sbin/squid`

![Pasted image 20240605233247](https://github.com/lm3nitro/Projects/assets/55665256/09425b3c-b2a7-4444-b757-3c940748e2e1)

> [!IMPORTANT]  
> All of Squid's files ar elocated in `/usr/local/squid/`
> ![Pasted image 20240606162127](https://github.com/lm3nitro/Projects/assets/55665256/b9e885c2-b408-4f3f-8c2d-2b7169dc7b9d)

## Starting Squid:

So far, I have downloaded and installed Squid, configured it, and generated the needed certificates. Next, I procced to start Squid:

```
sudo -u proxy -- /usr/local/squid/sbin/squid -z 
sudo -u proxy -- /usr/local/squid/sbin/squid -d 10
```

![Pasted image 20240605233617](https://github.com/lm3nitro/Projects/assets/55665256/602d96cd-7af8-47fc-8201-06c4620d83a2)

Checked the status of Squid's processes:

![Pasted image 20240605235057](https://github.com/lm3nitro/Projects/assets/55665256/446c3662-9129-442d-ab61-f6c9b8e65e58)

> [!TIP]
> Optional: Installing colors to logs:
> ```apt install ccze```
> ![Pasted image 20240605233802](https://github.com/lm3nitro/Projects/assets/55665256/d40b8869-c438-4059-a9b1-854ab932bf36)

Verified the cache log:

```
tail -f /usr/local/squid/var/logs/cache.log
```

![Pasted image 20240605234002](https://github.com/lm3nitro/Projects/assets/55665256/506e7742-47fb-48c7-b9cd-06623e685934)

Verified the access log:

> [!IMPORTANT]  
> There is no log currently. This is because I have not yet configured the Windows client with the proxy information. However, the command to check would be the following:
> ```tail -f /usr/local/squid/var/logs/access.log```

> [!NOTE]
> The following is optional: Installing calamaris
> Calamaris is a tool that works in conjunction with Squid Proxy to generate log analysis reports. It is specifically designed to analyze Squid's log files and produce reports that give insights into proxy usage, traffic patterns, performance metrics, and more.
>```apt search calamaris```
> ![Pasted image 20240605234421](https://github.com/lm3nitro/Projects/assets/55665256/e852737c-081b-4455-b808-da58410847be)
> ```apt install calamaris```
> ![Pasted image 20240605234408](https://github.com/lm3nitro/Projects/assets/55665256/84950571-000c-4427-820d-fbb5c9f3332f)

Next, I verified that Squid is listening:

```
ss -tlp
```

![Pasted image 20240605234757](https://github.com/lm3nitro/Projects/assets/55665256/560d7752-6409-4ec5-84c6-51b8a9cb4549)

```
ps -aux | grep 109564
```

![Pasted image 20240605235300](https://github.com/lm3nitro/Projects/assets/55665256/de0931ba-97a3-4503-9527-842a441937bc)

## Windows Client

After completing the configuration for Squid, I then procced to set-up my Windows 10 VM and configure it to use the proxy:

1. I first downloaded the certificates to the Windows client:

![Pasted image 20240605235638](https://github.com/lm3nitro/Projects/assets/55665256/84bddefe-778a-41fb-b646-6deb7cd50612)

![Pasted image 20240606000152](https://github.com/lm3nitro/Projects/assets/55665256/896346b5-200b-4f45-8942-d524aece9cd8)

2. I then verified the IP address of the Squid Proxy and entered the information into the Proxy Windows system settings:

![Pasted image 20240606000533](https://github.com/lm3nitro/Projects/assets/55665256/f2c9068c-6855-45ae-85f1-d53e1c59053e)

![Pasted image 20240606000605](https://github.com/lm3nitro/Projects/assets/55665256/c100b96a-08f2-4e1e-bcc8-d78917462f1e)

3. Once the information was entered, I verified the connection status from the Windows client:

![Pasted image 20240606002736](https://github.com/lm3nitro/Projects/assets/55665256/d58af6d2-984f-4b28-8e54-3dbe2e8661fd)

4. I also confirmed and verified the logs at the proxy:

![Pasted image 20240606002237](https://github.com/lm3nitro/Projects/assets/55665256/99ac4719-6163-48fb-b2e6-904e6bee1fd4)

## Test 1

To test, I generated traffic on the Windows client. I went to `lichess.org` to test:

![Pasted image 20240606004834](https://github.com/lm3nitro/Projects/assets/55665256/3d20665e-a764-410f-9df1-363f38f543ce)

Verified the certificate, confirmed that everything was working as expected:

![Pasted image 20240606010804](https://github.com/lm3nitro/Projects/assets/55665256/fe40ddec-5939-47e4-a4fa-2c7fb9de8cd7)

While testing, I also had Wireshark running in the background and was able to confirm the Windows client was indeed using the Squid Proxy:

![Pasted image 20240606003952](https://github.com/lm3nitro/Projects/assets/55665256/f5fe00e1-c607-46e4-83f9-ec7b35654d73)

Next, I went verified the traffic on the proxy side. Decrypted `Lichess.org` traffic at the proxy:

![Pasted image 20240606004112](https://github.com/lm3nitro/Projects/assets/55665256/dbed89ea-0ca6-45b6-838b-1f6a4c8497cb)

I then Decrypted the traffic to `lichess.org`on the Windows Client using Wireshark:

![Pasted image 20240606003503](https://github.com/lm3nitro/Projects/assets/55665256/04375c5b-a2e6-495c-a99c-09e87571bd0f)

After the traffic has been decrypted, it seems like `lichess.org` is using HTTP/1.1:

![Pasted image 20240606003915](https://github.com/lm3nitro/Projects/assets/55665256/f3b4bfe5-f2cf-43e2-a70a-0f747b932b90)

The version that Wireshark is showing also matches the version installed:

![Pasted image 20240606004627](https://github.com/lm3nitro/Projects/assets/55665256/0db23a17-b0a5-4d19-a22e-bd2cf1b35de1)

In order to further test, I exported HTTP objects from the `lichess.org` traffic for further analysis:

![Pasted image 20240606005510](https://github.com/lm3nitro/Projects/assets/55665256/28c0e1b6-de2e-420d-aedb-d9721ca42503)

Exported objects:

![Pasted image 20240606005706](https://github.com/lm3nitro/Projects/assets/55665256/643368fd-51c4-488c-be1b-b71137b681f3)

## Test 2

As another test, I wanted to test downloading and see how the traffic is seen when using the proxy. For this test, I downloaded Firefox:

![Pasted image 20240606011922](https://github.com/lm3nitro/Projects/assets/55665256/cd8cb294-8863-411f-8182-e8eb960e4c6c)

In the proxy logs I was able to see the installer.exe:

![Pasted image 20240606011518](https://github.com/lm3nitro/Projects/assets/55665256/5bb9cee9-91f8-4d9e-8601-183c444df57c)

Another part of this exercise that is not seen, is that I configured Squid Proxy logs to be sent to my Splunk instance. Below is the traffic showing the download of the installer.exe. 

![Pasted image 20240606093227](https://github.com/lm3nitro/Projects/assets/55665256/2e395fc1-a746-48cc-8113-5385d378d529)

I also verify that the Cache Squid is working. In the `squid.conf` under `/usr/local/squid/etc/` I create a cache rule to cache any of the files listed there:

![Pasted image 20240607174629](https://github.com/lm3nitro/Projects/assets/55665256/f8f6266d-ff4b-47bd-86ff-14dc429a7005)

> [!NOTE]  
> The squid cache is under /usr/local/squid/var/cache/
> ![Pasted image 20240607172049](https://github.com/lm3nitro/Projects/assets/55665256/a26372fa-0cfa-4dd5-b50d-75e63ccbc194)

Searched all the directories and files search recursively:

```
find . -type f | xargs grep -E  installer
```

![Pasted image 20240607172542](https://github.com/lm3nitro/Projects/assets/55665256/10638eb1-7c2a-4c80-8866-f80b866dbfb3)

Another way to search:

```
find .  -type f -print0 | xargs -0 grep -l "installer"
```

![Pasted image 20240607172414](https://github.com/lm3nitro/Projects/assets/55665256/007e2a88-77c0-4d1f-a1ef-98f1ffc32e75)

To check, I opened the last file with the following command:

```
nano ./squid/00/0E/00000EE4
```

![Pasted image 20240607172837](https://github.com/lm3nitro/Projects/assets/55665256/e9a211a1-349a-4c7e-9566-1efa26a28160)

Opened another file on the list 

```
nano squid/00/09/0000097A
```

I was able to get the actual installer.exe file binary!

![Pasted image 20240607173631](https://github.com/lm3nitro/Projects/assets/55665256/a840a659-2332-4f55-a45a-34ac9cac4b8a)

```
xxd squid/00/09/0000097A | grep -iE  'installer|exe|dos'
```

![Pasted image 20240607180003](https://github.com/lm3nitro/Projects/assets/55665256/1c0667c7-1b2d-4b02-b2c2-c8abb97611d6)

> [!NOTE]  
> Earlier, I installed `ccze` in order to colorize the logs. Below is a view at how the logs look with the color added. I find that this makes it more readable:
> ```cat  /usr/local/squid/var/logs/access.log | ccze -A -C -o noscroll | grep -iE "installer.exe"```
> ![Pasted image 20240606094234](https://github.com/lm3nitro/Projects/assets/55665256/b647bff2-dabf-431e-bffd-e62fe03997a3)

## Test 3

Next, I disabled the proxy setting on the Windows client:

![Pasted image 20240606100136](https://github.com/lm3nitro/Projects/assets/55665256/e9570f58-8652-47cf-93aa-3c2b2a342903)

It’s important to note that the Win API enables proxy access for all 'proxy-capable' applications on the operating system, allowing them to route traffic through the proxy.

Even though I disabled the proxy setting on the Windows client, I am still seeing traffic going to Microsoft even though I didn't browse to the Microsoft website:

![Pasted image 20240606110825](https://github.com/lm3nitro/Projects/assets/55665256/b742a48e-48a9-422d-9550-5513d79f3ce6)

In order to fix that, I installed a browser that doesn't use the Win API. I will then need to block all the traffic at the firewall level from this Windows client. Firefox has a separate API for the application and does not use Win API. I then configured the proxy settings in Firefox:

![Pasted image 20240606095903](https://github.com/lm3nitro/Projects/assets/55665256/7524db31-3b81-4c62-99c0-0c06e6863a93)

Once that was completed, I then created a rule in my Fortinet firewall. This firewall rule will allow only Squid proxy traffic to have access to the internet. In this situation the firewall is already allowing "All outbound". The only thing I needed to do was create a new entry to block the Windows Client PC on 10.10.100.101:

![Pasted image 20240606105235](https://github.com/lm3nitro/Projects/assets/55665256/b232cfac-ea1a-40d1-aef6-c42d7c496f27)

Looking at the hit count for the rule:

![Pasted image 20240606104922](https://github.com/lm3nitro/Projects/assets/55665256/8502c873-3214-42ce-819f-cd006d43bdbd)

I was able to check and see that the traffic coming from the Windows client was being blocked:

![Pasted image 20240606105941](https://github.com/lm3nitro/Projects/assets/55665256/408cd0d7-5a54-4df2-a013-c2985fcdaea9)

I also wanted to ensure that the traffic from the proxy was being allowed. First, I needed to confirm the IP address:

![Pasted image 20240606110515](https://github.com/lm3nitro/Projects/assets/55665256/d18409a9-8580-441b-9434-443206c6cf84)

Firewall logs allowing the Proxy traffic:

![Pasted image 20240606110439](https://github.com/lm3nitro/Projects/assets/55665256/c70d19a4-99a4-49e6-8cb8-727af352b520)

Now, the firewall is blocking all the traffic generated from the Windows client (IP 10.10.100.101) that is using Win API to go outbound and not passing through the proxy. By doing this, I am enforcing the proxy policy and using only the Firefox app to go outbound to the internet.

> [!NOTE]  
> Optional: Disabling Squid Cache:
> On the configuration file located under `/usr/local/squid/etc`, remove these lines:
> ```
> maximum_object_size 6 GB
> cache_mem 8192 MB
> cache_dir ufs /usr/local/squid/var/cache/squid 32000 16 256
> ```
> ![Pasted image 20240606162914](https://github.com/lm3nitro/Projects/assets/55665256/79a1786d-5844-440a-a4a7-c14f2c715ae8)
> You can also use the cache access list to make Squid never cache any response:
> ```cache deny all```

## Blocking Domain

Squid can block or allow requests based on the destination domain or hostname provided in the SSL handshake. This allows you to implement domain-based access controls, enabling the restriction of specific websites, content categories, or services.

Next, I wanted to test this functionality. To do this, I first created a domain blacklist in `Squid.conf` file. I edited the file located in `/etc/squid/squid.conf` and added the following settings at the beginning of the file: 

```
acl domain_blacklist dstdomain "/usr/local/squid/etc/domain_blacklist.txt"
http_access deny all domain_blacklist
```

![Pasted image 20240606170758](https://github.com/lm3nitro/Projects/assets/55665256/ceb80d25-8294-41a2-bc2e-925996c4b2e2)

I then created the `/usr/local/squid/etcdomain_blacklist.txt` file and added the domains I wanted to block. For example, to block access to `example.com`, including subdomains, add: 

```
.lm3nitro.example.com
lm3nitro.squid.lab.example.com
```

In my case, I will be blocking `lichess` and `chess.com`:

![Pasted image 20240606171228](https://github.com/lm3nitro/Projects/assets/55665256/63017f46-1347-4dae-b765-11fa9eaa6ae3)

I then killed the squid process:

![Pasted image 20240606171520](https://github.com/lm3nitro/Projects/assets/55665256/6ff49ebd-bc72-4174-ac81-70adfc1203ea)

I then started the squid service:

```
sudo -u proxy -- /usr/local/squid/sbin/squid -z
sudo -u proxy -- /usr/local/squid/sbin/squid -d 10
```

Verified the service again, I can see that I got new pid numbers:

![Pasted image 20240606171819](https://github.com/lm3nitro/Projects/assets/55665256/b40861f5-0c49-4ab8-be42-bf2adf24c885)

Testinf time! I attempted to go to `lichess.org` and `chess.com`:

![Pasted image 20240606170337](https://github.com/lm3nitro/Projects/assets/55665256/b903cb39-4f50-4442-9f84-93b6ff164062)

![Pasted image 20240606171917](https://github.com/lm3nitro/Projects/assets/55665256/626a8545-21e8-4039-a369-e93967a5186c)

I can see that the access was denied and that the domains were truly being blocked. I then also looked at Squid's logs and could also see that the traffic was getting blocked:

```
tail  -f /usr/local/squid/var/logs/access.log | grep -E  "lichess|chess"
```

![Pasted image 20240606172048](https://github.com/lm3nitro/Projects/assets/55665256/f3839693-4594-47fc-8442-2302562672d5)

I also tested trying to download the eicar file and was able to see that as well:

```
cat /usr/local/squid/var/logs/access.log | grep -E  ' eicar|zip'
```

![Pasted image 20240607182009](https://github.com/lm3nitro/Projects/assets/55665256/28439e70-540e-4562-9768-8c12f69b0afb)

## Monitor Process and Network Traffic:

In order to monitor the network traffic, I will be using `nload, htop and tcptrack`. 

+ nload: This is a command-line tool used to monitor and display network traffic in real-time on Linux and Unix-like systems. It provides a visual representation of incoming and outgoing network bandwidth usage on a system, making it easy to track network activity, diagnose potential issues, and assess network performance.

+ htop: This is an interactive process viewer that is widely used for monitoring system resources, managing processes, and providing insights into the overall health and performance of a system.

+ tcptrack: This command line tool allows you to view active TCP connections, along with detailed information about each connection, such as the source and destination IP addresses, ports, connection status, and data transfer statistics.

Installing nload:

```
apt isntall nload
```

![Pasted image 20240607184651](https://github.com/lm3nitro/Projects/assets/55665256/115c0a34-8e95-4a4a-8c75-87c244f6652f)

Installing htop

```
apt install htop
```

![Pasted image 20240607213149](https://github.com/lm3nitro/Projects/assets/55665256/1c01293f-c817-49b8-b96a-acd0655bea92)

Installing tcptrack

```
apt install tcptrack
```

![Pasted image 20240607213409](https://github.com/lm3nitro/Projects/assets/55665256/501d1ac9-fc76-4969-9ed2-e9f85a2a4b04)

A view of all the tools being used:

![Pasted image 20240607213757](https://github.com/lm3nitro/Projects/assets/55665256/db541c0a-7765-4ea5-a04a-90536adeb053)


## Securing Squid Proxy:

![Pasted image 20240607211638](https://github.com/lm3nitro/Projects/assets/55665256/b4a52da8-1258-439a-84ff-a502296058b6)

ClamAV is an open-source antivirus software that primarily uses signature-based detection to identify viruses, trojans, and other types of malware, and it supports scanning of files, email attachments, and archives. 

In order to better secure Squid Proxy, I will be installing ClamAV:

```
apt install clamav clamav-daemon
```

![Pasted image 20240607203552](https://github.com/lm3nitro/Projects/assets/55665256/cfbb49d9-b4d3-4102-a6da-83481032b366)

Verify version:

```
clamscan --version
```

![Pasted image 20240607203705](https://github.com/lm3nitro/Projects/assets/55665256/dda2a059-ce15-4596-b8fd-f6699e50b1b1)

Checking the status

```
systemctl status clamav-freshclam
```

![Pasted image 20240607203900](https://github.com/lm3nitro/Projects/assets/55665256/301a05de-fdc7-4154-b904-5f328ca43ce5)

I then stopped clamav in order to update the DB:

```
systemctl stop clamav-freshclam
```

![Pasted image 20240607204043](https://github.com/lm3nitro/Projects/assets/55665256/e7fdc4dc-8d18-42cf-ad9a-fe499791faf3)

To update the DB, I ran the following command:

```
freshclam
```

![Pasted image 20240607204139](https://github.com/lm3nitro/Projects/assets/55665256/7b6184b2-84a8-47e6-a896-99665682e1c4)

Next, I started ClamAV now that the DB was updated. I used `enable` for persistence:

```
systemctl enable clamav-freshclam --now
```

![Pasted image 20240607204328](https://github.com/lm3nitro/Projects/assets/55665256/dc8e349f-7be4-46a8-9cdc-ee1913c444d6)

> [!NOTE]  
> To disable ClamAV, use the following command:
> ```
> systemctl disable clamav-freshclam --now
> ```

I then verified the clamav files and directory:

```
ls -l /var/lib/clamav/
```

![Pasted image 20240607204513](https://github.com/lm3nitro/Projects/assets/55665256/f8d8d330-2b28-48ae-99d6-63dc1411b2df)

## Testing ClamAV:

To test, I attempted to download the antivirus test files. It probably won't encounter many viruses, as they have become rare. It is more likely to find other forms of malware like worms, backdoors, and ransomware.

![Pasted image 20240607210556](https://github.com/lm3nitro/Projects/assets/55665256/d6d4c558-fc6e-419e-8067-62cb15537baa)

![Pasted image 20240607210724](https://github.com/lm3nitro/Projects/assets/55665256/d265fb03-f72f-41f2-aa1a-9148c775f943)

Scanning current directory:

```
clamscan .
```

![Pasted image 20240607210918](https://github.com/lm3nitro/Projects/assets/55665256/a113e7d0-37ae-4961-9279-0716b3e932a3)

ClamAV was able to find the 2 infected files that were downloaded. 

### Summary:

This exercise provided me with hands-on experience with traffic interception, content filtering, and device hardening. I was able to learn how to manage and inspect encrypted traffic and block harmful domains by using a proxy. This experience also allowed me to better understand the role of proxy servers in network security, as well as learning to fine-tune configurations for optimal performance and safety. 

Below are some key benefits learned from this exercise and having this set up:

+ Traffic Visibility: SSL Bump allows you to decrypt and inspect HTTPS traffic
+ Performance Optimization: Squid Proxy provides caching capabilities that improves network performance
+ Security Hardening: Installing and using like ClamAV for hardening the proxy server, making it more resilient to attacks


