# Sysmon

Sysmon is Windows system service that monitors and logs system activity. It provides detailed information about process creations, network connections, and changes to file creation and modification times. Sysmon enabled administrators to detect and investigate malicious activities by providing comprehensive event logs. 

### Scope:
Sysmon specifically focuses on the domain of endpoint security. In this project I will be installing and configuring Sysmon on a Win 10 OS. The second part of the project will be installing the Splunk Forwarder in order to send Sysmon logs and events to Splunk for further monitoring. 

### Tools and Technology:
Windows OS, PowerShell, Sysmon, and Splunk

## Installation:

These are the steps I used to get Sysmon installed and running on my system. First, I will need to download Sysmon directly from Microsoft. Once downloaded, I will also need to download the sysmonconfig in xml format. I have provided the configuration file I will be using seperately in **SysmonConfig.xml**.

<img width="940" alt="Screenshot 2024-04-29 at 9 21 07 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/cd59df2f-8ba1-48b9-802a-f3314d5e9050">

Once I downloaded the configuration file, I navigated to where Sysmon was downloaded, right-clicked on the zip file, and selected *Extract File*. 

<img width="1121" alt="Screenshot 2024-04-29 at 9 22 43 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/973cef41-07e8-4c47-893a-d2523cb22133">

Once that was extracted, I needed to ensure that the extracted files along with my configuration file were all in the same location as seen below:

<img width="891" alt="Screenshot 2024-04-29 at 9 33 55 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b81601c9-d52e-41c3-bcd6-c1763772155e">

Next, I will need to open PowerShell as Administrator and go to the directory where I have the Sysmon installation and configuration files.

<img width="862" alt="Screenshot 2024-04-29 at 9 38 14 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/bf159400-1a94-479e-8b50-032293ed039b">

I then installed it via the following command to point to out configuration file:

```
.\Sysmon64.exe -i sysmonconfig.xml
```

<img width="857" alt="Screenshot 2024-04-29 at 9 41 55 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d45752c6-138d-491c-bc28-3c3e39337a8b">

Once the install started, I got prompted with the license agreement, click *Agree*

<img width="857" alt="Screenshot 2024-04-29 at 9 43 01 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/15f898f0-1297-4a17-945f-4f9f715158e9">

Once that completed, I could see that Sysmon had started:

<img width="859" alt="Screenshot 2024-04-29 at 9 43 42 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/f64b715c-17ab-4214-b263-ef56c3a3e3df">

For verification, I also navigated to services and saw that it was running:

<img width="962" alt="Screenshot 2024-04-29 at 9 45 20 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/6c2ad16f-23ef-473e-88f8-47b73d02c684">

As a quick test to ensure it was logging information, I navigated to chess.com, and was able to verify in Event Viewer that it was getting logged by Sysmon:

<img width="1100" alt="Screenshot 2024-04-29 at 9 58 35 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/12e6801d-7f0b-4f6a-9502-8d3b48362a5b">

### Summary:

I have installed Sysmon using the configuration file. Next, I will need to install the Splunk forwarder. This can be seen under **Send Sysmon logs to Splunk**.
