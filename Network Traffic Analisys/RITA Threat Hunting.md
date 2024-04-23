

# Threat Hunting:


![Pasted image 20240417142556](https://github.com/lm3nitro/Projects/assets/55665256/6b14a118-e487-4ae2-937f-fdde7d621eea)



# Running PCAP with Suricata:

![Pasted image 20240417141729](https://github.com/lm3nitro/Projects/assets/55665256/39e3b253-c90f-45a1-93f5-a6fbc2003e61)






Alerts from 67.207.93.135 :


![Pasted image 20240417142306](https://github.com/lm3nitro/Projects/assets/55665256/cd6548be-378f-4999-91ed-4f9ccaa49975)

![Pasted image 20240417142154](https://github.com/lm3nitro/Projects/assets/55665256/567ecc81-9889-4bd4-838a-f3962dc74068)



# Using jq to analyse Eve.json logs:

Installation:

apt install jq

![Pasted image 20240417152300](https://github.com/lm3nitro/Projects/assets/55665256/c782b717-f6a1-4661-a247-c30439bb75c1)


cat eve.json| jq  -C 'select(.dest_ip== "67.207.93.135")'

![Pasted image 20240417152740](https://github.com/lm3nitro/Projects/assets/55665256/3cdf5c78-b1b7-44e0-a4f8-a42e3978261a)


![Pasted image 20240417153204](https://github.com/lm3nitro/Projects/assets/55665256/521c3597-96e6-4518-af34-be321fb2c000)




# Wireshark:

![Pasted image 20240417160509](https://github.com/lm3nitro/Projects/assets/55665256/7c80cc70-611a-477f-bab0-94e94a7345c3)

![Pasted image 20240417174216](https://github.com/lm3nitro/Projects/assets/55665256/fbb15dda-8ca0-45c6-8b52-c6aa66907fab)


![Pasted image 20240417155611](https://github.com/lm3nitro/Projects/assets/55665256/0e4b9d13-9539-4176-8e53-a80900e7f733)



##User Agent Malware Information:
![Pasted image 20240417161921](https://github.com/lm3nitro/Projects/assets/55665256/e640ad7a-a63f-452e-bc0a-6a8ed86aa04e)




## Windows 10 user agent: 

![Pasted image 20240417162733](https://github.com/lm3nitro/Projects/assets/55665256/e30511fc-6381-4503-bc7a-b722417cbec2)


![Pasted image 20240417175130](https://github.com/lm3nitro/Projects/assets/55665256/21e399cd-5653-43d3-8011-f46bf11475c6)


![Pasted image 20240417160242](https://github.com/lm3nitro/Projects/assets/55665256/01a20155-b75f-4da7-b131-bebd3adca4e6)


##DNS Blacklist: 

![Pasted image 20240417154159](https://github.com/lm3nitro/Projects/assets/55665256/109dea0c-2490-40a2-8ab0-946967ea45d2)



# Rita Commands:

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



# Zeek Commands: 


### Reading the PCAP file with Zeek

![Pasted image 20240417135108](https://github.com/lm3nitro/Projects/assets/55665256/50aa2c9b-ca16-4fcf-ac4d-dae08d53651c)

![Pasted image 20240417134208](https://github.com/lm3nitro/Projects/assets/55665256/0e9984d8-8587-45e0-9e7d-67fed739cbd3)

# Import Zeek logs into Rita:

![Pasted image 20240417134605](https://github.com/lm3nitro/Projects/assets/55665256/a14c0cc7-5be2-43a3-ab90-5deea7d4bddf)

# Show Beacons:

![Pasted image 20240417134934](https://github.com/lm3nitro/Projects/assets/55665256/45c59f7e-380d-42ab-be23-a494578a97c7)

# Creating a HTML report:

![Pasted image 20240417135439](https://github.com/lm3nitro/Projects/assets/55665256/bffc26f5-7dc4-43ad-bc99-5736ee7e3e7f)

![Pasted image 20240417135702](https://github.com/lm3nitro/Projects/assets/55665256/66e239cd-90bc-4593-a821-87a86124536c)

![Pasted image 20240417140109](https://github.com/lm3nitro/Projects/assets/55665256/a38b4c7d-1773-43b3-b920-39d5c5d3f18b)

![Pasted image 20240417141343](https://github.com/lm3nitro/Projects/assets/55665256/ab66b468-4467-4ff0-b596-337a8fef45b9)

![Pasted image 20240417141224](https://github.com/lm3nitro/Projects/assets/55665256/3b9b7f0c-9320-4c65-8cba-ff660bc82e86)

![Pasted image 20240417140607](https://github.com/lm3nitro/Projects/assets/55665256/74e5d89d-745e-4353-9000-432f858707b4)

![Pasted image 20240417140812](https://github.com/lm3nitro/Projects/assets/55665256/5c3bffdf-cdb0-4c34-bdc0-b600bf87ed7f)


![Pasted image 20240417141057](https://github.com/lm3nitro/Projects/assets/55665256/e00488c5-2467-4fd2-a680-cc8b5486ab3d)



