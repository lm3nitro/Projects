# Threat Hunting 

## Scenario:

You are a cybersecurity analyst working for a multinational corporation that has recently experienced a series of suspicious network activities, including unusual data transfers. You are tasked with conducting a comprehensive threat hunt using a 24-hour PCAP file captured from a critical network segment. Your goal is to leverage the combined capabilities of RITA, Zeek, and Suricata on a dedicated server to analyze the PCAP data, identify potential threats or indicators of compromise (IOCs), and strengthen the organization's overall cybersecurity posture.

This is the 24hr pcap given to us to investigate:

![Pasted image 20240417142556](https://github.com/lm3nitro/Projects/assets/55665256/6b14a118-e487-4ae2-937f-fdde7d621eea)

### Running PCAP with Suricata:

I start off by analyzing the pcap with Suricata. Suricata will inspect the traffic for known attack signatures and unusual network behaviors, generating alerts for suspicious activities such as port scans, malware downloads, or communication with known malicious IPs.

![Pasted image 20240417141729](https://github.com/lm3nitro/Projects/assets/55665256/39e3b253-c90f-45a1-93f5-a6fbc2003e61)

Upon the analysis with Suricata we see the following alerts from the IP 67.207.93.135. The "Invalid Checksum: Generic Protocol Command Decode" normally means that Suricata decoded and interpreted a command within the generic protocol. This could include commands related to various network protocols like HTTP, FTP, DNS, or other application-layer protocols. 

Also, its important to note the Invalid Checksum. Checksums are used to verify the integrity of data packets. An "Invalid Checksum" message suggests that the checksum value doesn't match the expected value, which could mean a potential data integrity issue.

![Pasted image 20240417142306](https://github.com/lm3nitro/Projects/assets/55665256/cd6548be-378f-4999-91ed-4f9ccaa49975)

Although Suricata doesn't look to have a specific rule or signature for this type of traffic, there is an abnormal amount of alerts which warrants for further analysis. Let's filter for this IP only and see how many hits we get in the fast.log:

```
cat fast.log | grep 67.207.93.135 | WC -l
```
![Pasted image 20240417142154](https://github.com/lm3nitro/Projects/assets/55665256/567ecc81-9889-4bd4-838a-f3962dc74068)

### Using jq to analyse Eve.json logs:

Next, I want to analyze the eve.json logs using jq. This is command-line tool used for parsing and manipulating JSON data. It allows you to filter, extract, transform, and format JSON objects and arrays

Installation:
```
apt install jq
```
![Pasted image 20240417152300](https://github.com/lm3nitro/Projects/assets/55665256/c782b717-f6a1-4661-a247-c30439bb75c1)

```
cat eve.json| jq  -C 'select(.dest_ip== "67.207.93.135")'
```
This will allow me to see more information about the traffic and conversation, the suricata alert that was triggered, flow-id, and so much more. 

![Pasted image 20240417152740](https://github.com/lm3nitro/Projects/assets/55665256/3cdf5c78-b1b7-44e0-a4f8-a42e3978261a)

![Pasted image 20240417153204](https://github.com/lm3nitro/Projects/assets/55665256/521c3597-96e6-4518-af34-be321fb2c000)


### Wireshark

Now that I have more information on what we are looking for, we can take a look at the pcap with Wireshark. Here I took a look at the protocol hierarchy to get a high level overview of the traffic and protocols seen in the pcap. 

![Pasted image 20240417160509](https://github.com/lm3nitro/Projects/assets/55665256/7c80cc70-611a-477f-bab0-94e94a7345c3)

I then wanted to narrow in on that specific IP address we saw above (67.207.93.135) and see the traffic, bytes transferred, IPs communicating with is, etc. Here I can see that there was 677MB sent to our internal host from this IP address:

![Pasted image 20240417174216](https://github.com/lm3nitro/Projects/assets/55665256/fbb15dda-8ca0-45c6-8b52-c6aa66907fab)

Next I filtered in on the conversation between our internal host and this IP address and see some HTTP traffic that gives us more details regarding the host, URI, and the http method used:

![Pasted image 20240417155611](https://github.com/lm3nitro/Projects/assets/55665256/0e4b9d13-9539-4176-8e53-a80900e7f733)

Now that I have information about the user agent, I went to look it up and see what information I could find. 

### User Agent Lookup

When I looked up the user agent, I was provided with the following information. I can see that this user agent is referencing a platform that is not used within the environment, in this case Windows XP. 

![Pasted image 20240417161921](https://github.com/lm3nitro/Projects/assets/55665256/e640ad7a-a63f-452e-bc0a-6a8ed86aa04e)

With this information I went to take another look at the pcap. Here I am seeing traffic from the same internal host regarding Microsoft's Delivery Optimization.

![Pasted image 20240417162733](https://github.com/lm3nitro/Projects/assets/55665256/e30511fc-6381-4503-bc7a-b722417cbec2)

I wanted to take a closer look into Microsoft's Delivery Optimization and the client it utilizes. Based in this information, we can see that it only references Windows 10 and Windows 11. This further emphasizes that the internal host is not a XP windows agents. 

![Pasted image 20240417175130](https://github.com/lm3nitro/Projects/assets/55665256/21e399cd-5653-43d3-8011-f46bf11475c6)

I then filtered specifically for the http GET method and this suspicious IP and I can see the the traffic happens to come in intervals:

![Pasted image 20240417160242](https://github.com/lm3nitro/Projects/assets/55665256/01a20155-b75f-4da7-b131-bebd3adca4e6)

### DNS Blacklist 

At this point I wanted to take a closer look at the host highlighted in the screenshot above:

![Pasted image 20240417154159](https://github.com/lm3nitro/Projects/assets/55665256/109dea0c-2490-40a2-8ab0-946967ea45d2)

### Zeek: 

Now that I have the above information, I wanted to read the pcap with Zeek, and then import the Zeek logs to RITA to see what more information I am able to find. When reading the PCAP file with Zeek, these are some of the command options available:

![Pasted image 20240417135108](https://github.com/lm3nitro/Projects/assets/55665256/50aa2c9b-ca16-4fcf-ac4d-dae08d53651c)

These are the logs provided by Zeek:

![Pasted image 20240417134208](https://github.com/lm3nitro/Projects/assets/55665256/0e9984d8-8587-45e0-9e7d-67fed739cbd3)

Next, I imported the Zeek logs into Rita:

![Pasted image 20240417134605](https://github.com/lm3nitro/Projects/assets/55665256/a14c0cc7-5be2-43a3-ab90-5deea7d4bddf)


### Rita:

Below are some RITA commands that are very useful:

     clean, clean-databases   Finds and removes broken databases. Prompts before deleting each database unless --force is provided.
     delete, delete-database  Delete imported database(s)
     import                   Import zeek logs into a target database
     html-report              Create an html report for an analyzed database
     show-beacons-proxy       Print hosts which show signs of C2 software (internal -> Proxy)
     show-beacons-sni         Print hosts which show signs of C2 software (SNI Analysis)
     show-beacons             Print hosts which show signs of C2 software
     show-bl-hostnames        Print blacklisted hostnames which received connections
     show-bl-source-ips       Print blacklisted IPs which initiated connections
     show-bl-dest-ips         Print blacklisted IPs which received connections
     list, show-databases     Print the databases currently stored
     show-dns-fqdn-ips        Print IPs associated with FQDN via DNS
     show-exploded-dns        Print dns analysis. Exposes covert dns channels
     show-ip-dns-fqdns        Print FQDNs associated with IP Address via DNS
     show-long-connections    Print long connections and relevant information
     show-open-connections    Print open connections and relevant information
     show-strobes             Print strobe information
     show-useragents          Print user agent information
     test-config              Check the configuration file for validity
     help, h                  Shows a list of commands or help for one command

Commands specific to beacons:

![Pasted image 20240417134934](https://github.com/lm3nitro/Projects/assets/55665256/45c59f7e-380d-42ab-be23-a494578a97c7)

Once the Zeek logs were imported to RITA, I then created a HTML report:

![Pasted image 20240417135439](https://github.com/lm3nitro/Projects/assets/55665256/bffc26f5-7dc4-43ad-bc99-5736ee7e3e7f)

![Pasted image 20240417135702](https://github.com/lm3nitro/Projects/assets/55665256/66e239cd-90bc-4593-a821-87a86124536c)

Look at how many beacons it was able to pick up. In this case, I was very curious about the suspicious IP:

![Pasted image 20240417140109](https://github.com/lm3nitro/Projects/assets/55665256/a38b4c7d-1773-43b3-b920-39d5c5d3f18b)

It was also able to locate the suspicious IP in its DB:

![Pasted image 20240417141343](https://github.com/lm3nitro/Projects/assets/55665256/ab66b468-4467-4ff0-b596-337a8fef45b9)

![Pasted image 20240417141224](https://github.com/lm3nitro/Projects/assets/55665256/3b9b7f0c-9320-4c65-8cba-ff660bc82e86)

I then navigated to the source URL where it was reported, this case threatfox. This provided us with a lot more information and even provided us with reference to the malware, in this case Colbalt Strike and the threat type, in this case botnet_cc. 

![Pasted image 20240417140607](https://github.com/lm3nitro/Projects/assets/55665256/74e5d89d-745e-4353-9000-432f858707b4)

I also went a step further to reference URL mentioned in the screenshot above:

![Pasted image 20240417140812](https://github.com/lm3nitro/Projects/assets/55665256/5c3bffdf-cdb0-4c34-bdc0-b600bf87ed7f)

This now provided us with very useful information such as IOCs, Alerts, file name, file type, and so much more:

![Pasted image 20240417141057](https://github.com/lm3nitro/Projects/assets/55665256/e00488c5-2467-4fd2-a680-cc8b5486ab3d)

### Summary: 

In summary, after conducting a thorough threat hunting investigation utilizing Zeek, Suricata, RITA, and Wireshark, suspicious HTTP GET traffic originating from the 67.207.93.135 IP address was identified. Upon analyzing further, I was able to see that this IP was associated with malicious activity related to malware. To prevent incidents in the future, it is crucial to incorporate IOCs into the security infrastructure and regularly update threat intelligence feeds. I also believe that if an EDR is not yet being used in the environment, implementing and deploying an EDR solution for real-time monitoring would be highly beneficial.

There are also steps needed to be taken since we were able to confirm the internal host was communicating with this malicious IP. Some of the steps that can be taken are:

+ Isolate the host
+ Perform forensic analysis on the host
+ Remove the malware if infected
+ Patch and update the host
+ Review Security policies (Firewall, access controls, etc)
+ Update the IOCs
+ Educate users
+ Review and update incident response plan as needed


