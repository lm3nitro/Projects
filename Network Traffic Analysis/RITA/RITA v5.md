# RITA V5

RITA(Real Intelligence Threat Analytics), is a cybersecurity tool that helps organizations analyze and detect potential security threats in their networks. It uses advanced algorithms to process data and identify unusual patterns or behaviors that could indicate a security breach or attack. 

![Pasted image 20240914110553](https://github.com/user-attachments/assets/3c450f17-2b6d-4519-8afa-533061e903b0)

Compatibility:

![Pasted image 20240914115025](https://github.com/user-attachments/assets/9e35e0f0-2fbf-43a9-b159-50c8388426d9)

### Scope:

In this lab, I will be installing RITA and will be using it to analyze a pcap file containing suspicious network traffic. After RITA processes the pcap, I will be utilizing other tools such as Wireshark, VirusTotal, etc. to further analyze the malicious behavior. After completing this analysis, I will provide a summary of the findings, detailing the identified threats, their implications, and recommended actions for remediation. 

### Tools and Technology:
Ubuntu, Docker, Rita, Zeek, Virus Total, Wireshark, and CyberChef

## Installation:

I will be installing Rita v5 on my Ubuntu 24.04 VM. Below are the system specs:

![Pasted image 20240914115133](https://github.com/user-attachments/assets/30d1503c-2faf-4be9-99ce-f7fcf7eedb7b)

Also important to note is the network interface that I will be using (ens32):

![Pasted image 20240914114135](https://github.com/user-attachments/assets/73392058-6e0d-4832-9ef7-985e867f5eb3)

I used the following link to download the latest release of the code:

```
https://github.com/activecm/rita/releases
```

I copied the following link and downloaded the script:

![Pasted image 20240914114925](https://github.com/user-attachments/assets/f6e1751d-6739-40eb-a5b6-5d47ecee8ebc)

![Pasted image 20240914113623](https://github.com/user-attachments/assets/bd8e362f-8ecd-4c3e-a4d8-9b80f629de72)

Once the script was downloaded, I changed the permissions:

![Pasted image 20240914113709](https://github.com/user-attachments/assets/a63794a4-9d8d-4a06-9f6d-a3d270fc16be)

I then executed the script:

```
sudo ./install-rita-zeek-here.sh
```

![Pasted image 20240914113822](https://github.com/user-attachments/assets/df1bac69-01c6-422e-bd3f-2d2d8248f5f8)

During the installation it prompted for a BECOME password:

![Pasted image 20240914114026](https://github.com/user-attachments/assets/9742dadc-cf6e-4282-903d-38093e431bcb)

Snip of the installation process.......

![Pasted image 20240914114218](https://github.com/user-attachments/assets/4e78a8e7-bac3-4340-9bc5-56ce582cdc3c)

> [!TIP]
> Use the `arrows` to go down and `space bar` to select.

As noted above, I will be configuring to use network interface `ens32`

![Pasted image 20240914114333](https://github.com/user-attachments/assets/b3dca26c-097c-42af-95c1-7822571fcafe)

Confirmed the capturing interface:

![Pasted image 20240914114401](https://github.com/user-attachments/assets/423d3989-cb69-4d84-a4fb-091bd47a2bf2)

Next, I confirmed that everything was installed by verifying the docker version, images, and status:

```
docker -v
```

![Pasted image 20240914115613](https://github.com/user-attachments/assets/a29538e0-bae7-4f3c-99bf-0c178f8f44ad)

```
sudo docker images
```

![Pasted image 20240914114645](https://github.com/user-attachments/assets/378f0b68-d7d1-4d9f-9c45-79f03a4db4b4)

```
sudo docker ps
```

![Pasted image 20240914114721](https://github.com/user-attachments/assets/6189d652-7b36-4174-84af-09bf20b21b7a)

## Zeek:

After installing Rita and Zeek, I then began to analyze the pcap (zeus_24hr.pcap). 

Before getting started, I gathered the statistics of the file:

```
capinfos zeus_24hr.pcap
```

![Pasted image 20240914120933](https://github.com/user-attachments/assets/cbdc4242-aff2-442c-afd2-9bf422476cad)

Based on the output, this is a 24-hour capture. I then processed the pcap using Zeek:

```
zeek readpcap ~/Documents/pcaps/zeus_24hr.pcap ~/Documents/zeek-logs/
```

These were the logs that Zeek was able to generate:

![Pasted image 20240914121409](https://github.com/user-attachments/assets/b02f1acf-e6d7-4dfe-a598-6a117a869116)

## RITA

After using Zeek, I then imported the Zeek logs to Rita:

> [!TIP]
> Getting help using Rita:
> `rita -h`
> ![Pasted image 20240914122601](https://github.com/user-attachments/assets/c47e757b-0ab2-4b40-bcb3-35e32db9c8be)

```
rita import -l zeek-logs/ -d hunting
```

![Pasted image 20240914122742](https://github.com/user-attachments/assets/079d86f2-2256-45a6-a93d-23afb48ac70e)

After the logs had been imported, I verified the database:

![Pasted image 20240915131312](https://github.com/user-attachments/assets/2dda5a68-6d9e-4743-b794-2f139e195ad9)

I then viewed what was generated inside the database:

```
rita view hunting
```

![Pasted image 20240914123316](https://github.com/user-attachments/assets/99aca490-17a9-45ab-b5a1-83335002d6f6)

Based on ouput below, the highlighted FQDN was flagged as critical. 

![Pasted image 20240914123303](https://github.com/user-attachments/assets/7d09fd46-c433-452a-8065-2cde4b8f5202)

## VirusTotal

After being provided with the FQDN above, I investigated further using VirusTotal:

![Pasted image 20240914123526](https://github.com/user-attachments/assets/0ff56d64-f804-4949-a363-0d19b4d55e6f)

This is being flagged as malicious. 

## Zeek Cutter

In order to further analyze the logs, I used Zeek Cutter utility to read the Zeek logs generated earlier. I first had to download and install it:

```
mkdir /home/lm3nitro/Documents
cd /home/lm3nitro/Documents
wget https://raw.githubusercontent.com/activecm/zcutter/main/zcutter.py -O zcutter.py
```

![Pasted image 20240914125044](https://github.com/user-attachments/assets/4af2479e-6ca7-4cf0-8e44-fa80d3882630)

Changed permissions:

```
chmod 755 zcutter.py
if ! type zeek-cut >/dev/null 2>&1 ; then ln -s zcutter.py zeek-cut ; fi
```

![Pasted image 20240914125229](https://github.com/user-attachments/assets/a6ca3f96-33b1-4644-ba4f-a1e02f2afa53)

Once installed, I searched in the http.logs for the FQDN flagged by Rita (mahamaya1ifesciences.com user-agent: mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; SV1):

```
cat http.log | ~/Documents/tools/zcutter.py | head
```

![Pasted image 20240914130859](https://github.com/user-attachments/assets/2323a41b-14fb-4ca0-8a52-1467504c7943)

The domain mahamaya1ifesciences.com is mapped to the IP 67.207.93.135, it seems like the internal IP downloaded an image file from the server at  /metro91/admin/1/ppptp.jpg

## Wireshark:

After identifying the IP address, I then filtered for this IP address using Wireshark. It seems that the internal host at 192.168.99.53 was in constant communication with the external IP 67.207.93.135 for 24 hours:

![Pasted image 20240914132522](https://github.com/user-attachments/assets/39f6c072-7092-4659-845b-7b2274084dcf)

I then inspected the payload using Wireshark. It seems that the image is encoded in base64 and compressed in Gzip, also there is some PowerShell code attached to the payload making it able to run in memory:

![Pasted image 20240914124705](https://github.com/user-attachments/assets/d58e18f1-32b1-46dc-a166-1aecd502970a)

## CyberChef

I then copied the encoded payload and decoded the payload using CyberChef:

![Pasted image 20240914124448](https://github.com/user-attachments/assets/ce10519b-f298-4da3-9dce-7896b7ecd996)

## Findings

Based on the analysis done above and the investigation, below is the summary of my findings:

+ Malware: Zeus
+ Connection Type: Reverse HTTP
+ C2 Platform: Cobalt Strike
+ Host Payload Delivery Method: PowerShell one-liner
+ Target Host/Victim: 192.168.99.53
+ C2 Server: 67.207.93.135
+ Beacon: 24 Hours

### Summary:

In summary, the analysis of the pcap file confirmed malicious activity (C2 communication),leading to a deeper understanding of the attack patterns and techniques employed by adversaries. By utilizing a variety of tools-Rita for threat detection, Zeek for logging, Wireshark for packet inspection, VirusTotal for threat verification, and CyberChef for decoding—an extensive and accurate analysis was achieved. The findings highlighted the importance of using different tools to gain a comprehensive understanding of the threat landscape. 


