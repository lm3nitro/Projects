


![Pasted image 20240601171802](https://github.com/lm3nitro/Projects/assets/55665256/b37821c9-fad7-44ed-9a62-e39e84e0d7f4)







Pi-hole is a network-wide ad blocker that functions as a Domain Name System (DNS) sinkhole. It effectively filters out unwanted content, such as ads, trackers, and malicious domains, at the DNS level, without installing any client-side software. 


 Pi-hole provides a few key benefits:

Ad Blocking: Pi-hole prevents ads from loading on websites, which not only speeds up page loading times but also enhances the overall browsing experience.

Privacy: It blocks trackers and third-party analytics, reducing the amount of data collection by various online services and improving your privacy.

Security: Pi-hole blocks access to known malicious domains, thereby protecting your network from potentially harmful content.

Network Performance: With fewer ads and tracking requests, your network's performance can improve, especially if you have multiple devices connected.





Network Diagram:



![Pasted image 20240601175714](https://github.com/lm3nitro/Projects/assets/55665256/5f774e33-cc43-490f-8d57-46e47ee5fa65)




Information:


https://www.wundertech.net/use-unbound-to-enhance-the-privacy-of-pi-hole-on-a-raspberry-pi/

# Installation:  



Clone our repository and run:


git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
cd "Pi-hole/automated install/"
sudo bash basic-install.sh




![Pasted image 20240602112549](https://github.com/lm3nitro/Projects/assets/55665256/ece21b67-a5d7-4a84-8f71-c0a0eacc47e9)



![Pasted image 20240602112742](https://github.com/lm3nitro/Projects/assets/55665256/ed22f987-f511-49b5-8e67-f2ab4b37bd79)





![Pasted image 20240602112804](https://github.com/lm3nitro/Projects/assets/55665256/ccec05cd-1ac8-45f5-9ce9-e33fa44b580e)



Verity the IP address:



![Pasted image 20240602113850](https://github.com/lm3nitro/Projects/assets/55665256/5f67a965-a21c-4a0e-97c7-d1e9b04233d5)




Netplan network configuration:


```
network:
  ethernets:
    ens192:
      addresses:
      - 10.10.100.35/24
      nameservers:
        addresses:
        - 10.10.100.1
        search: []
      routes:
      - to: default
        via: 10.10.100.1
  version: 2
```

Screenshot of Netplan network configuration:

![Pasted image 20240602114005](https://github.com/lm3nitro/Projects/assets/55665256/6c65cd85-2d2a-4163-8b80-fe297cd838ce)


![Pasted image 20240602112914](https://github.com/lm3nitro/Projects/assets/55665256/a8c0436a-c4fc-42df-a44d-89d757fe768e)



![Pasted image 20240602113004](https://github.com/lm3nitro/Projects/assets/55665256/0a98546f-15b3-4548-b9e1-4bb5b6338f93)




![Pasted image 20240602113036](https://github.com/lm3nitro/Projects/assets/55665256/e604419e-61fb-4fe7-98e8-880bf35a1905)




![Pasted image 20240602113057](https://github.com/lm3nitro/Projects/assets/55665256/e638d7d6-fb09-495d-a12e-183c62636d14)




![Pasted image 20240602113129](https://github.com/lm3nitro/Projects/assets/55665256/bd491ed8-a1a1-40f9-aa0d-2ed3cfdfb830)



![Pasted image 20240602113213](https://github.com/lm3nitro/Projects/assets/55665256/c92f7fa1-bd80-4dd9-8fa8-6af646c93ad1)




![Pasted image 20240602113247](https://github.com/lm3nitro/Projects/assets/55665256/9a0e32e8-e2cf-4786-a030-035150741ce7)


![Pasted image 20240602113645](https://github.com/lm3nitro/Projects/assets/55665256/339f15cd-53a3-4b78-8716-c6c0d988006f)






# Setting up Unbound:


Installing Unbound:



sudo apt install unbound


![Pasted image 20240602114230](https://github.com/lm3nitro/Projects/assets/55665256/ad781f37-2a96-4fdb-bc58-ed80d4ee1ec1)

```
systemctl start unbound.service
systemctl status unbound.service
systemctl restart unbound.service
systemctl enable unbound.service
```




Downloading the root.hints file:

wget https://www.internic.net/domain/named.root -qO- | sudo tee /var/lib/unbound/root.hints


![Pasted image 20240602114448](https://github.com/lm3nitro/Projects/assets/55665256/120b1fab-9a3d-48e3-b39f-cb886108aa26)


Create a file that will force Unbound to only listen for queries from Pi-hole:


nano /etc/unbound/unbound.conf.d/pi-hole.conf



![Pasted image 20240602114832](https://github.com/lm3nitro/Projects/assets/55665256/86913d8e-ca79-4883-af09-eddf1be7c43e)


File:

```
server:
    # If no logfile is specified, syslog is used
    # logfile: "/var/log/unbound/unbound.log"
    verbosity: 0
    interface: 127.0.0.1
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes
    # May be set to yes if you have IPv6 connectivity
    do-ip6: no
    # You want to leave this to no unless you have *native* IPv6. With 6to4 and
    # Terredo tunnels your web browser should favor IPv4 for the same reasons
    prefer-ip6: no
    # Use this only when you downloaded the list of primary root servers!
    # If you use the default dns-root-data package, unbound will find it automatically
    #root-hints: "/var/lib/unbound/root.hints"
    # Trust glue only if it is within the server's authority
    harden-glue: yes
    # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
    harden-dnssec-stripped: yes
    # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
    # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
    use-caps-for-id: no
    # Reduce EDNS reassembly buffer size.
    # Suggested by the unbound man page to reduce fragmentation reassembly problems
    edns-buffer-size: 1472
    # Perform prefetching of close to expired message cache entries
    # This only applies to domains that have been frequently queried
    prefetch: yes
    # One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
    num-threads: 1
    # Ensure kernel buffer is large enough to not lose messages in traffic spikes
    so-rcvbuf: 1m
    # Ensure privacy of local IP ranges
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
```




Restart the Unbound server and check the status of the service: 
```
sudo service unbound restart

sudo service unbound status
```


![Pasted image 20240602115022](https://github.com/lm3nitro/Projects/assets/55665256/a3cd851f-54c9-44e4-b98b-dcf388c58003)


Testing the resolution of the DNS:


dig pi-hole.net @127.0.0.1 -p 5335



![Pasted image 20240602115221](https://github.com/lm3nitro/Projects/assets/55665256/2f69c24a-1ee9-46ec-937b-a12b7152af4f)




Verify the Unbound and Pi-hole port  are listening :

![Pasted image 20240602123258](https://github.com/lm3nitro/Projects/assets/55665256/b3bc00e9-89be-416b-bf1d-bda62ffa3344)




# Login to WebUI:


![Pasted image 20240602115808](https://github.com/lm3nitro/Projects/assets/55665256/8d209133-d732-4095-8b5c-6c58f7b279eb)




Setting up the unbound local IP and port:


![Pasted image 20240602120115](https://github.com/lm3nitro/Projects/assets/55665256/3e0379c5-f45d-488b-8f55-0b9068afabd5)




Click Save



Configure Static DNS on Windows 10 PC:


![Pasted image 20240602120330](https://github.com/lm3nitro/Projects/assets/55665256/7b02d80a-5f4e-4d9c-bfc1-9ef0b23d4f10)




Let's go to chess.com website to test the blocking adds:

![Pasted image 20240602120739](https://github.com/lm3nitro/Projects/assets/55665256/e598122a-98a4-49ea-90dc-458a88a21037)




Network traffic on the Pi-hole server received from Windows PC:

![Pasted image 20240602122855](https://github.com/lm3nitro/Projects/assets/55665256/ed4b2099-68b6-474a-a6a9-81bd04873085)



DNS traffic going to the Local DNS Pi-Hole server from Windows PC:


![Pasted image 20240602122206](https://github.com/lm3nitro/Projects/assets/55665256/6d03a22f-1043-4f11-82a6-b360413ab4ea)



Click on Dashboard:

![Pasted image 20240602120845](https://github.com/lm3nitro/Projects/assets/55665256/494259e6-4e93-4ef2-93f3-f0cffe3a71c2)



Click on Query logs:


![Pasted image 20240602120821](https://github.com/lm3nitro/Projects/assets/55665256/bf7e73a6-ed78-4a98-abd8-76c236619973)



# Check the efficiency of the Pi-hole ad blocker:



https://d3ward.github.io/toolz/adblock


![Pasted image 20240602121056](https://github.com/lm3nitro/Projects/assets/55665256/5fa08427-aa7b-43ca-b94a-72cf14462468)


We have generated more traffic:



![Pasted image 20240602121148](https://github.com/lm3nitro/Projects/assets/55665256/0256be17-7d2b-43d0-a90d-647926d58cf7)


![Pasted image 20240602121238](https://github.com/lm3nitro/Projects/assets/55665256/f5198fd6-3dad-4973-b738-0b5bc226cf91)




Looking DNS at statistics:


![Pasted image 20240602121943](https://github.com/lm3nitro/Projects/assets/55665256/8db98278-faf0-4d4b-b6c8-d76a16e2da6a)






# Add custom Blocklist to Pi-Hole:

Note:

The StebenBlacklist was added by the Pi-hole installer during the installation:


https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts




Let's add the tracking list:

https://blocklistproject.github.io/Lists/tracking.txt


![Pasted image 20240602132330](https://github.com/lm3nitro/Projects/assets/55665256/266b925f-f78d-4069-8479-734e2dcb7ffb)

![Pasted image 20240602132541](https://github.com/lm3nitro/Projects/assets/55665256/39567e29-3b77-4e3c-90c5-20e6ba7d056b)




# Downlofing a Big blocklist:


https://firebog.net/

![Pasted image 20240602133658](https://github.com/lm3nitro/Projects/assets/55665256/fe7d9117-17dd-4ef9-a465-845b873c5d62)




+ https://blocklistproject.github.io/Lists/scam.txt
+ https://blocklistproject.github.io/Lists/malware.txt
+ https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt
+ https://raw.githubusercontent.com/kboghdady/youTube_ads_4_pi-hole/master/youtubelist.txt
+ https://raw.githubusercontent.com/danhorton7/pihole-block-tiktok/main/tiktok.txt




# Activating the blacklist to Pi-hole:


![Pasted image 20240602134018](https://github.com/lm3nitro/Projects/assets/55665256/97fd7390-1f9f-4a89-9468-8c73707e0011)





pihole -g

![Pasted image 20240602134104](https://github.com/lm3nitro/Projects/assets/55665256/38d9d4f6-9529-497b-a801-0a9491a55412)


Now our lists vas a green checkmark:

Malware blocklist:


+ https://osint.digitalside.it/Threat-Intel/lists/latestdomains.txt
+ https://v.firebog.net/hosts/RPiList-Malware.txt
+ https://v.firebog.net/hosts/RPiList-Phishing.txt

![Pasted image 20240602134605](https://github.com/lm3nitro/Projects/assets/55665256/b64741db-cb44-485f-ac7a-d5282c93041c)



# Blocking a ramdom domain:

Let's block youtube.com


![Pasted image 20240602135024](https://github.com/lm3nitro/Projects/assets/55665256/507dc31b-0d5c-4817-9131-dc3da51417e9)




Verify if youtube.com is really blocked:

We can see the DNS traffic, the DNS server (Pi-hole) 10.10.100.35  is responding with 0.0.0.0, we also can see the DNS queries from the Pi-hole on the right side:



![Pasted image 20240602135447](https://github.com/lm3nitro/Projects/assets/55665256/521a4fb3-63df-4e34-8d2b-967c8a2c8748)



# Logs analysis:

![Pasted image 20240602150946](https://github.com/lm3nitro/Projects/assets/55665256/04bf4347-a5be-4198-b73a-23e0164ab3fb)



Making a DNS query to chess.com:


Looking at real-time DNS queries with tail -f  pihole.log

![Pasted image 20240602150922](https://github.com/lm3nitro/Projects/assets/55665256/00b8d3dd-7e13-4082-b755-caa51bb21bd6)




# Data filtering with grep:

Looking the last 10 DNS queries to chess.com

sudo cat pihole.log | grep chess.com | tail -n 10


![Pasted image 20240602165119](https://github.com/lm3nitro/Projects/assets/55665256/0fea8470-3fa5-408e-8479-27ef54640fe2)


Looking the first 10 queries to chess.com domain:

sudo cat pihole.log | grep chess.com | head -n 10

![Pasted image 20240602165211](https://github.com/lm3nitro/Projects/assets/55665256/7b4634ec-0829-44dc-8927-9f338a684f88)



Counting the DNS calls that contains the domain chess.com:

sudo cat pihole.log | grep chess.com | wc -l

![Pasted image 20240602165539](https://github.com/lm3nitro/Projects/assets/55665256/ed6d433a-af6d-4fc0-bb55-eaaa5f0edebf)


showing the count of the different fields that contain the domain chess.com

![Pasted image 20240602171644](https://github.com/lm3nitro/Projects/assets/55665256/91fe78c1-6917-4cc4-aedb-593f3514ed15)



Total Queries made to chess.com:

![Pasted image 20240602171239](https://github.com/lm3nitro/Projects/assets/55665256/6241ee50-33fa-4190-a26e-62780fdf8378)



count for all dns calls that start with lh and is followed by any digit and a period and other words at the tail end
sudo cat pihole.log | grep -Po 'lh\d\.\w+' | wc -l

Same as above, but without the count, this shows that actual results. We can see that all start with lh and is followed by either a 5,4, or 3.

sudo cat pihole.log | grep -Po 'lh\d\.\w+'


![Pasted image 20240602172727](https://github.com/lm3nitro/Projects/assets/55665256/08cc0ae9-6910-402e-b369-a2cd6bfef404)

Summary:
