# $${\color{lightblue}Nessus}$$

<img width="310" alt="Screenshot 2024-05-18 at 12 20 46 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/b03da507-1696-40ca-b410-d604e5783e8b">

Nessus is a security scanning tool, which scans a computer and raises an alert if it discovers any vulnerabilities that malicious hackers could use to gain access to any computer you have connected to a network.

Vulnerability Scanning is the process of detecting, assessing, and reporting on security defects and vulnerabilities. Automated vulnerability scanning technologies are used to access the attack surface, identify potential risk exposures and attack vectors throughout an organization’s networks, hardware, software, and systems. The scanning and assessment of vulnerabilities is a critical phase in the vulnerability management lifecycle.

### Scope:
I will download and install Nessus and will run vulnerability assessment on a target host.

### Tools and Technologies:
Nessus, Wireshark, Nmap, Linux OS, Metasploitable

## Installation
You can download direct from the browser or from the CLI via curl:

Downloading via web browser:

![Pasted image 20240419145946](https://github.com/lm3nitro/Projects/assets/55665256/02825edc-c3eb-48f4-9e36-f1c8cf619df0)

Downloading via curl:

![Pasted image 20240419150136](https://github.com/lm3nitro/Projects/assets/55665256/951e644b-353c-417f-a6c9-977b7de20047)

Install with dpkg: 

![Pasted image 20240419162531](https://github.com/lm3nitro/Projects/assets/55665256/9a3687d6-de15-4b3a-a664-eb596c0633d1)

Once installed, update and upgrade all packages:

```
sudo apt upgrade && upgrade
```

![Pasted image 20240419145938](https://github.com/lm3nitro/Projects/assets/55665256/95030125-1496-4e79-9772-ecfffb2440e0)

## Start or Stop Nessus:

The following commands can be used to start or stop Nessus:

```
systemctl start nessusd
systemctl stop nessusd
```
Now that it is installed, lets launch it:

![Pasted image 20240419152225](https://github.com/lm3nitro/Projects/assets/55665256/84a9fa7c-46b0-49fc-a978-fecd8cda6237)

Upon logging in, we are presented with plugin, license, and component updates:

![Pasted image 20240419152543](https://github.com/lm3nitro/Projects/assets/55665256/892e425f-0aed-4b9e-bb59-9dc06988d2f1)

## Basic Network Scan:

Once we have set up Nessus, we can create and perform a basic network scan. A Basic Network Scan will perform similarly to an Advanced Network Scan. The only difference being that Advanced Network Scans have the following features/attributes:

- Allows the fine-tuning of the plugins included in the scan
- Has some different default settings
- Allows audits to be added to it

The Basic Network Scan is a great for the average network scan. The Advanced Network Scan allows for a lot more customization to tailor fit a policy against a host or range of hosts

Navigate to *My Scans > New Scan*
![Pasted image 20240419153718](https://github.com/lm3nitro/Projects/assets/55665256/ae4d56fd-054a-449b-938e-287dff851781)

Here are the different scanning options:

![Pasted image 20240419153859](https://github.com/lm3nitro/Projects/assets/55665256/d40c7a45-6ac2-461c-b319-97539a36e630)

I selected Basic Network Scan. Upon choosing, you are presented with a dialog box to enter the details. I will be scanning against 1 target host:

![Pasted image 20240419154030](https://github.com/lm3nitro/Projects/assets/55665256/82fdbc15-731a-4268-8f07-12c545c9c187)

To initiate the scan, select the *play* icon:

![Pasted image 20240419154135](https://github.com/lm3nitro/Projects/assets/55665256/040663ac-01df-45fc-b51f-b203c794b05c)

This is a brief overview of the vulnerabilities found and a view of the scan details:

![Pasted image 20240419161227](https://github.com/lm3nitro/Projects/assets/55665256/11362f7c-b661-40bb-9ede-b60e0c3639f8)

## Vulnerability Metrics

The Common Vulnerability Scoring System (CVSS) is a method used to supply a qualitative measure of severity. CVSS is not a measure of risk. CVSS consists of three metric groups: Base, Temporal, and Environmental. The Base metrics result in a numerical score ranging from 0 to 10, which can then be modified by assessing the Temporal and Environmental metrics. A CVSS assessment is also represented as a vector string, a compressed textual representation of the values used to derive the score. The higher the score the more critical/severe the vulnerability is.

![Pasted image 20240419161218](https://github.com/lm3nitro/Projects/assets/55665256/12d69150-da0d-4984-b2da-da1c7b37bcac)

Based on the results, the scanner was able to find a Bind shell on the host: 

>#### Simply a Bash shell that is _bind_ to port `1524/tcp` will run everything sent to that port on Bash and reply with the output. You don't need tools like Metasploit for that; a simple Netcat or Telnet will do.

We can see that within the details of the findings, it also provides a solution if the host has been compromised:

![Pasted image 20240419161439](https://github.com/lm3nitro/Projects/assets/55665256/27af5c6e-7fc5-40f6-adc3-055b9a5dee09)

## Target Host

I then went to the target host and wanted to dive deeper with Nmap into this hidden shell:

![Pasted image 20240419164019](https://github.com/lm3nitro/Projects/assets/55665256/f702d761-c5e5-421b-bfc7-92bbd12bedf8)

Based on the output, it looks like the vulnerability is a True Positive and it does exist on our host:

![Pasted image 20240419164118](https://github.com/lm3nitro/Projects/assets/55665256/7e6d5170-68a7-457b-b557-ce3741203ea0)

Looking at the traffic on the host with Wireshark, we can see that it is indeed open:

![Pasted image 20240419164615](https://github.com/lm3nitro/Projects/assets/55665256/92ff8a12-53ae-453d-8474-f08f0c67027a)

I then tried connecting to the Shell using Netcat:

```
netcat 192.168.44.136 1524
```

![Pasted image 20240419165419](https://github.com/lm3nitro/Projects/assets/55665256/195433bf-a3b0-4fab-9103-be0ff94e2393)

I was able to connect as root. I checked Wireshark again as it was still running in the background and could see the traffic:

![Pasted image 20240419165508](https://github.com/lm3nitro/Projects/assets/55665256/93ddee58-e221-4d99-b297-d12809f3b13d)

## Remediation: 

Vulnerability remediation is the process of finding, addressing, and neutralizing security vulnerabilities which can include computers, digital assets, networks, web applications, and mobile devices. It's one of the most important steps in the vulnerability management cycle, which is critical for securing networks, preventing data loss, and enforcing business continuity.

### Summary: 

Nessus is a great tool for running series of vulnerability scans and test against devices. I liked the granularity that it provided when it came to the details of the vulnerabilities it was able to find. In this scan, we were able to see the backdoor that was present on the host, we also tested to see if we could exploit it and were successful. We also analyzed the traffic in Wireshark to see what the exploit looks like when executed against the vulnerability. 

Overall, there are a few things that can be done to help in minimizing vulnerabilities, these include the following:
+ Regular Software Upgrades
+ Strong Access Controls
+ Network Segmentation
+ Regular Vulnerability Scanning and Penetration Testing
+ Employee Training

These are just a few, there are many more. 

