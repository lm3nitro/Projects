# Pi-hole

![Pasted image 20240601171802](https://github.com/lm3nitro/Projects/assets/55665256/b37821c9-fad7-44ed-9a62-e39e84e0d7f4)

Pi-hole is a network-wide ad blocker that functions as a Domain Name System (DNS) sinkhole, which intercepts and blocks ads and trackers at the network level. It helps to improve network performance by preventing unwanted traffic and enhances privacy by blocking domains. 

Pi-hole provides a few key benefits:

+ Network-Wide Ad Blocking: Pi-hole prevents ads from loading on websites, which not only speeds up page loading times but also enhances the overall browsing experience.
+ Enhanced Privacy: It blocks trackers and third-party analytics, reducing the amount of data collection by various online services and improving your privacy.
+ Security: Pi-hole blocks access to known malicious domains, thereby protecting your network from potentially harmful content.
+ Network Performance: With fewer ads and tracking requests, your network's performance can improve, especially if you have multiple devices connected.

### Scope:

I will be installing Pi-hole on a Linux OS host and will be testing both the ad-blocking and domain blocking capabilities that Pi-hole has to offer. To do this, I will be using a Win 10 OS that will be configured to use the Pi-hole and where I will generate traffic. I will also be testing its efficiency and analyzing the generated traffic on the Pi-hole web interface, Wireshark, and Pi-hole logs themselves. 

### Tools and Technology:

Linux OS, Windows OS, Pi-hole, Unbound and Wireshark

### Network Diagram:

![Pasted image 20240601175714](https://github.com/lm3nitro/Projects/assets/55665256/5f774e33-cc43-490f-8d57-46e47ee5fa65)

Information:
https://www.wundertech.net/use-unbound-to-enhance-the-privacy-of-pi-hole-on-a-raspberry-pi/

## Installation:  
To get started, I first verified the IP address and Netplan configuration. This is important information to know prior to installing and getting Pi-hole setup. 

Verify the IP address:

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

Once I had the information needed, I was ready to install Pi-hole. I will be installing it on a Linux VM. First, I cloned the repository and ran the installer script:

```
git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
cd "Pi-hole/automated install/"
sudo bash basic-install.sh
```

![Pasted image 20240602112549](https://github.com/lm3nitro/Projects/assets/55665256/ece21b67-a5d7-4a84-8f71-c0a0eacc47e9)

Once I ran the install script, I was then taken to this blue screen for the setup and configuration on Pi-hole:

![Pasted image 20240602112742](https://github.com/lm3nitro/Projects/assets/55665256/ed22f987-f511-49b5-8e67-f2ab4b37bd79)

The first 2 slides were informational and provided details about Pi-hole and the donations that help this initiative

![Pasted image 20240602112804](https://github.com/lm3nitro/Projects/assets/55665256/ccec05cd-1ac8-45f5-9ce9-e33fa44b580e)

This next slide is about the needs for a static IP on the OS where Pi-hole will be installed. Prior to starting this install, I verified the IP address (10.10.100.35) and ensured that it statically assigned. 

![Pasted image 20240602112914](https://github.com/lm3nitro/Projects/assets/55665256/a8c0436a-c4fc-42df-a44d-89d757fe768e)

I was them prompted to select a DNS provider. I chose Cloudflare:

![Pasted image 20240602113004](https://github.com/lm3nitro/Projects/assets/55665256/0a98546f-15b3-4548-b9e1-4bb5b6338f93)

I then agreed to using the third-party domain list:

![Pasted image 20240602113036](https://github.com/lm3nitro/Projects/assets/55665256/e604419e-61fb-4fe7-98e8-880bf35a1905)

I also agreed to installing the Admin Web Interface:

![Pasted image 20240602113057](https://github.com/lm3nitro/Projects/assets/55665256/e638d7d6-fb09-495d-a12e-183c62636d14)

The next slide is about the prerequisites required for the Admin Web Interface:

![Pasted image 20240602113129](https://github.com/lm3nitro/Projects/assets/55665256/bd491ed8-a1a1-40f9-aa0d-2ed3cfdfb830)

Query logging allows Pi-hole to keep a detailed log of all the DNS requests. I enabled it when getting it set-up:

![Pasted image 20240602113213](https://github.com/lm3nitro/Projects/assets/55665256/c92f7fa1-bd80-4dd9-8fa8-6af646c93ad1)

FTL privacy allows you to limit the amount of logging and data storage when it comes to the DNS queries. For this instance, I chose to go with **Show everything**

![image](https://github.com/lm3nitro/Projects/assets/55665256/9a5b2b04-c8c6-4a71-997e-bc234d4945a2)

This is the final slide to confirm the configuration:

![Pasted image 20240602113645](https://github.com/lm3nitro/Projects/assets/55665256/339f15cd-53a3-4b78-8716-c6c0d988006f)

## Setting up Unbound

Unbound is a recursive caching DNS resolver that is commonly used with Pi-hole to enhance DNS security and performance. I will be using it with Pi-hole to have it serve as the backend resolver for the DNS queries. 

To get started, I need to install unbound:
```
sudo apt install unbound
```
![Pasted image 20240602114230](https://github.com/lm3nitro/Projects/assets/55665256/ad781f37-2a96-4fdb-bc58-ed80d4ee1ec1)

These are the commands used to stop, restart, start, and check the service status:

```
systemctl start unbound.service
systemctl status unbound.service
systemctl restart unbound.service
systemctl enable unbound.service
```

Because unbound will be the recursive DNS resolver, I needed to download the root.hints file. This file contains the names and IP addresses of the authoritive name servers for the root zone. 

```
wget https://www.internic.net/domain/named.root -qO- | sudo tee /var/lib/unbound/root.hints
```

![Pasted image 20240602114448](https://github.com/lm3nitro/Projects/assets/55665256/120b1fab-9a3d-48e3-b39f-cb886108aa26)

Next, I created a file that will force Unbound to only listen for queries from Pi-hole:

```
nano /etc/unbound/unbound.conf.d/pi-hole.conf
```
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
Once the file was created, I restarted the Unbound server and checked the status of the service: 

```
sudo service unbound restart
sudo service unbound status
```

![Pasted image 20240602115022](https://github.com/lm3nitro/Projects/assets/55665256/a3cd851f-54c9-44e4-b98b-dcf388c58003)

After checking that status I then tested the DNS resolution:

```
dig pi-hole.net @127.0.0.1 -p 5335

```

![Pasted image 20240602115221](https://github.com/lm3nitro/Projects/assets/55665256/2f69c24a-1ee9-46ec-937b-a12b7152af4f)

I also verified that Unbound and Pi-hole ports were listening:

![Pasted image 20240602123258](https://github.com/lm3nitro/Projects/assets/55665256/b3bc00e9-89be-416b-bf1d-bda62ffa3344)

## Login to WebUI:

Once Pi-hole was installed and configured and Unbound was also installed, I then went to the Pi-hole Admin Web Interface:

![Pasted image 20240602115808](https://github.com/lm3nitro/Projects/assets/55665256/8d209133-d732-4095-8b5c-6c58f7b279eb)

First thing was setting up the unbound local IP and port:

![Pasted image 20240602120115](https://github.com/lm3nitro/Projects/assets/55665256/3e0379c5-f45d-488b-8f55-0b9068afabd5)

Click Save

## Testing and Analysis

For this project, I will be testing on a Windows 10 PC. To test I had to configure the static DNS:

![Pasted image 20240602120330](https://github.com/lm3nitro/Projects/assets/55665256/7b02d80a-5f4e-4d9c-bfc1-9ef0b23d4f10)

Time to test! I went to the chess.com website to test and see if it was truly blocking ads:

![Pasted image 20240602120739](https://github.com/lm3nitro/Projects/assets/55665256/e598122a-98a4-49ea-90dc-458a88a21037)

This is the network traffic on the Pi-hole server that it received from the Windows 10 PC:

![Pasted image 20240602122855](https://github.com/lm3nitro/Projects/assets/55665256/ed4b2099-68b6-474a-a6a9-81bd04873085)

In Wireshark I could also see DNS traffic going to the Local DNS Pi-Hole server from the Windows 10 PC:

![Pasted image 20240602122206](https://github.com/lm3nitro/Projects/assets/55665256/6d03a22f-1043-4f11-82a6-b360413ab4ea)

Going back to Pi-hole, I clicked on the dashboard and was able to see blocks:

![Pasted image 20240602120845](https://github.com/lm3nitro/Projects/assets/55665256/494259e6-4e93-4ef2-93f3-f0cffe3a71c2)

By clicking on Query logs, I was able to see where the ads were being blocked:

![Pasted image 20240602120821](https://github.com/lm3nitro/Projects/assets/55665256/bf7e73a6-ed78-4a98-abd8-76c236619973)

# Pi-hole Efficiency

The following website can be used to test the efficiency Pi-hole has on blocking ads:

https://d3ward.github.io/toolz/adblock

When using the website, I could see that there were ads getting blocked:

![Pasted image 20240602121056](https://github.com/lm3nitro/Projects/assets/55665256/5fa08427-aa7b-43ca-b94a-72cf14462468)

Upon checking Pi-hole, the testing using the website above indeed generated more traffic:

![Pasted image 20240602121148](https://github.com/lm3nitro/Projects/assets/55665256/0256be17-7d2b-43d0-a90d-647926d58cf7)

![Pasted image 20240602121238](https://github.com/lm3nitro/Projects/assets/55665256/f5198fd6-3dad-4973-b738-0b5bc226cf91)

Pi-hole also allows you to look at the DNS statistics. Here we can see the top domains that are getting blocked and the hosts that are generating the most traffic:

![Pasted image 20240602121943](https://github.com/lm3nitro/Projects/assets/55665256/8db98278-faf0-4d4b-b6c8-d76a16e2da6a)

## Adding a Custom Blocklist to Pi-Hole:

>#### Note: The StevenBlacklist was added by the Pi-hole installer during the installation:
>https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts

Since the StevenBlackLidt was already added during the installation, I will be adding the tracking list:
```
https://blocklistproject.github.io/Lists/tracking.txt
```
To add it, go to Add File, and from there select the downloaded files and select Add:

![Pasted image 20240602132330](https://github.com/lm3nitro/Projects/assets/55665256/266b925f-f78d-4069-8479-734e2dcb7ffb)

Here is a view at the list:

![Pasted image 20240602132541](https://github.com/lm3nitro/Projects/assets/55665256/39567e29-3b77-4e3c-90c5-20e6ba7d056b)

Aside from the above, I also downloaded other blocklists. You can find a group of blocklists by going to the following website:
```
https://firebog.net/
```

![Pasted image 20240602133658](https://github.com/lm3nitro/Projects/assets/55665256/fe7d9117-17dd-4ef9-a465-845b873c5d62)

These are the URLs for the blocklists:

+ https://blocklistproject.github.io/Lists/scam.txt
+ https://blocklistproject.github.io/Lists/malware.txt
+ https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt
+ https://raw.githubusercontent.com/kboghdady/youTube_ads_4_pi-hole/master/youtubelist.txt
+ https://raw.githubusercontent.com/danhorton7/pihole-block-tiktok/main/tiktok.txt

The following command can be used to update the gravity list to include the newly added domains in the downloaded lists:

![Pasted image 20240602134018](https://github.com/lm3nitro/Projects/assets/55665256/97fd7390-1f9f-4a89-9468-8c73707e0011)

```
pihole -g
```

![Pasted image 20240602134104](https://github.com/lm3nitro/Projects/assets/55665256/38d9d4f6-9529-497b-a801-0a9491a55412)

Now that the gravity list was updated, when I checked Pi-hole, the newly downloaded lists are now showing with a green checkmark:

Malware blocklist:
+ https://osint.digitalside.it/Threat-Intel/lists/latestdomains.txt
+ https://v.firebog.net/hosts/RPiList-Malware.txt
+ https://v.firebog.net/hosts/RPiList-Phishing.txt

![Pasted image 20240602134605](https://github.com/lm3nitro/Projects/assets/55665256/b64741db-cb44-485f-ac7a-d5282c93041c)

## Blocking a Domain:

This will be just an example. For this example, I will be using youtube.com. To do this, I went to Domains, and added youtube.com including subdomains:

![Pasted image 20240602135024](https://github.com/lm3nitro/Projects/assets/55665256/507dc31b-0d5c-4817-9131-dc3da51417e9)

I then verified to see if youtube.com was really blocked. We can see the DNS traffic, the DNS server (Pi-hole) 10.10.100.35 is responding with 0.0.0.0, we also can see the DNS queries from the Pi-hole on the right side:

![Pasted image 20240602135447](https://github.com/lm3nitro/Projects/assets/55665256/521a4fb3-63df-4e34-8d2b-967c8a2c8748)

## Logs analysis:

I also wanted to see the logs and what information they provide. 

![Pasted image 20240602150946](https://github.com/lm3nitro/Projects/assets/55665256/04bf4347-a5be-4198-b73a-23e0164ab3fb)

To generate traffic, I made a DNS query to chess.com. I was able to validate that DNS query, and the IP address associated with the FQDN

Real-time DNS queries with tail -f  pihole.log:

![Pasted image 20240602150922](https://github.com/lm3nitro/Projects/assets/55665256/00b8d3dd-7e13-4082-b755-caa51bb21bd6)


## Data filtering with grep:

When looking at the logs, I decided to comb through and filter for queries:

1. Looking the last 10 DNS queries to chess.com
   
```
sudo cat pihole.log | grep chess.com | tail -n 10
```

![Pasted image 20240602165119](https://github.com/lm3nitro/Projects/assets/55665256/0fea8470-3fa5-408e-8479-27ef54640fe2)


2. Looking the first 10 queries to chess.com domain:
   
```
sudo cat pihole.log | grep chess.com | head -n 10
```

![Pasted image 20240602165211](https://github.com/lm3nitro/Projects/assets/55665256/7b4634ec-0829-44dc-8927-9f338a684f88)

3. Counting the DNS calls that contains the domain chess.com:

```
sudo cat pihole.log | grep chess.com | wc -l
```

![Pasted image 20240602165539](https://github.com/lm3nitro/Projects/assets/55665256/ed6d433a-af6d-4fc0-bb55-eaaa5f0edebf)

4. Showing the count of the different fields that contain the domain chess.com

```
sudo cat pihole.log | grep -e chess.com |grep -o repy | wc -l
```

![Pasted image 20240602171644](https://github.com/lm3nitro/Projects/assets/55665256/91fe78c1-6917-4cc4-aedb-593f3514ed15)

5. Total Queries made to chess.com:

```
sudo cat pihole.log | grep -e chess.com |grep -e query
```

![Pasted image 20240602171239](https://github.com/lm3nitro/Projects/assets/55665256/6241ee50-33fa-4190-a26e-62780fdf8378)

6. Count for all DNS calls that start with lh and is followed by any digit and a period and other words at the tail end

```
sudo cat pihole.log | grep -Po 'lh\d\.\w+' | wc -l
```

7. Same as above, but without the count, this shows that actual results. We can see that all start with lh and is followed by either a 5,4, or 3.

```
sudo cat pihole.log | grep -Po 'lh\d\.\w+'
```

![Pasted image 20240602172727](https://github.com/lm3nitro/Projects/assets/55665256/08cc0ae9-6910-402e-b369-a2cd6bfef404)

### Summary:

Pi-hole is a valuable tool for enhancing network and user experience by blocking ads and tracks at the DNS level. This allowed me to get hand-on experience with DNS. While getting PI-hole setup and configured, it allowed me to better understand how DNS queries and resolution works. This project allowed me to me learn about DNS-level ad and tracker blocking. This also enhanced my awareness of network security and privacy practices. Analyzing the query logs offered insights into data privacy and exposure. 

It's important to have tools such as Pi-hole to help protect networks against threats and maintain privacy. Complementing Pi-hole with tools such as Unbound (as seen in this project), Suricata, and other technologies such as VPNs for additional privacy, helps to provide a more robust solution. 
