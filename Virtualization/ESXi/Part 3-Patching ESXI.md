# Patching ESXi

<img width="450" alt="Screenshot 2024-04-27 at 2 16 43 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8f091f17-b283-4730-b8ab-b0354b543b95">

### Scope:
This is Part 3. In Part 2 I installed and configured VMWare ESXi 7 on my server. In Part 2 I performed a vulnerability scan and found that there were some vulnerabilities that needed to be addresses. Here I will go through the process of pathing those vulnerabilities. Its important to note that I do not have any VMs running in my ESXi instance at this point. If VMs are running, ensuring that they are turned off is critical prior to starting the patching process. Upon applying the remediation, I will perfomring another scan to validate that the patch was applied and the CVEs remediated. 

### Tools and Technology:
VMWare ESXi and Tenable 

## Downloading Patch

My VMWare is currently running version 7.0 Update 2. This is where I went to verify the version that was currently installed and running:

<img width="1228" alt="Screenshot 2024-04-22 at 11 41 03 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/f94c41b1-c98e-4716-8eb5-06e0fd0de3a8">

To start, I needed to go to the VMWare patch downloads page, sign-in, and locate and download the patch:

<img width="669" alt="Screenshot 2024-04-22 at 11 36 53 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/3d870e41-1ade-400e-8b0d-83587a19515a">

In the downloads page, I am filtering for that version (7) and locating the last released patch. I selected the highled patch below and downloaded it. This is also the same patch that was referneced in the vulernability scan under the CVE as a remediation solution. 

<img width="1397" alt="Screenshot 2024-04-22 at 11 39 51 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/dcf17391-365f-4e3f-ada2-229ee8569bf8">

## Applying Patch

Once I had the downlaoded patch, I then went back to my ESXi server and navigated to **Storage > datastore1**:

<img width="1111" alt="Screenshot 2024-04-22 at 11 44 12 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/bf1ffd20-576a-4440-9dff-34fac9e368e0">

Here, it is important to make note of this **location**. This location will be used later on in the command used to install the patch:

<img width="1118" alt="Screenshot 2024-04-22 at 11 45 13 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/2ab3736d-de39-4d47-bb71-a97c8dfff585">

Once I had the location copied, I then selected **Datastore browser** which then opens a new dialog:

<img width="1337" alt="Screenshot 2024-04-22 at 1 05 20 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/f2b8b78f-1c10-42fd-a85d-3d95a10476e8">

Here I selected **upload**, and selected the newly downloaded patch that I had previosly downloaded from the vendor:

<img width="1350" alt="Screenshot 2024-04-22 at 1 07 57 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a25b3972-fae7-4e75-8946-1aba487c3b28">

Once that has been completed, we can ensure that we see the patch there, and click **Close** :

<img width="1329" alt="Screenshot 2024-04-22 at 1 09 53 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/9767760e-3189-4250-afae-ea812fed4f1e">

Next, click on **Host**

<img width="1022" alt="Screenshot 2024-04-22 at 1 12 22 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a185e59e-2d9a-49ac-a5e5-71784bc21860">

Under **Host** click on **Actions**

<img width="1094" alt="Screenshot 2024-04-22 at 1 13 14 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/3a9fe331-8c41-4df5-b3a6-1555096a9988">

We will need to enable SSH on our server so that we can SSH into it and perform the command to get the patch that we just uploaded installed:

<img width="1254" alt="Screenshot 2024-04-22 at 1 14 28 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/1bfd0849-8b69-4da0-8bee-eacafa7b1957">

Once I enabled SSH, I SSH'd into the server:

<img width="574" alt="Screenshot 2024-04-22 at 1 17 17 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b01875d3-d41b-421a-831e-40b654d15942">

Here is the command used install the patch. Root is needed to perform this step. Please keep in my that this command will change depending on the name of the patch that is being installed. Once the command is entered, it does take a couple of minutes to get nay response/output, this is normal and expected. 

```
esxcli software profile update -p ESXi-7.0U3p-23307199-standard -d /vmfs/volumes/66267610-5a7f0187-51c5-246e966417b8/VMware-ESXi-7.0U3p-23307199-depot.zip --no-hardware-warning 
```

Once it completes, we can see that it was successfully installed:

<img width="927" alt="Screenshot 2024-04-22 at 1 29 14 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/1181f576-9576-4f38-b6a5-ff13d9fe1a12">

Once we have the patch installed, we need to reboot the server to ensure that everything is applied. Its also important to note that once the server reboots, SSH will automatically be disabled again by default.

<img width="1472" alt="Screenshot 2024-04-22 at 1 55 18 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/7484e6be-bc8a-4d2c-96b5-d668f6dc2ea9">

Again, I do not have any VM's here at the time of this patch installation, so I can reboot without going into maintenance mode: 

<img width="1472" alt="Screenshot 2024-04-22 at 1 55 56 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/55fe1801-85d5-4096-a95d-a39546699717">

Once the server rebooted, I was able to see that the interface had changed which was a good sign that the patch had indeed been applied. 

<img width="993" alt="Screenshot 2024-04-22 at 2 02 51 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/e22d5685-1613-42af-9d03-19d2f2241fa9">

Upon logging in, we can also see that not only did the interface appearance change, but it now says version 7.0 update 3, which means that the patch got installed. 

<img width="1497" alt="Screenshot 2024-04-22 at 2 03 36 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/dcd17c5b-3d94-48a2-b688-337f3e143ec5">

## Post Remediation Vulnerability Scan

Now that the patch has been applied, I wanted to run another scan to ensure that the vulnerability was indeed remediated. While performing the scan, I was able to go to the ESXi server and monitor the traffic. Here we see some anomolies and a high volume that is espected due to the scan that we are performing:

![Pasted image 20240422141913](https://github.com/lm3nitro/Projects/assets/55665256/15dc49b0-3439-438a-bbfb-3aa832da72e5)

Once the scan completed, these were the results:

![Pasted image 20240422150130](https://github.com/lm3nitro/Projects/assets/55665256/7e6e359f-ec1e-43e0-a660-399ad4cbb68d)

The patch was applied and the vulnerabilities remediated. I am no longer seeing the Critical and High vulnerabilities. The Medium rated vulnerabilities are related to the certificate on the ESXi server: 

![Pasted image 20240422150211](https://github.com/lm3nitro/Projects/assets/55665256/d8d793f9-a11c-427d-bef7-60510db27cb2)

Here are the details of the Medium vulnerability related to the certificate:

![Pasted image 20240422150511](https://github.com/lm3nitro/Projects/assets/55665256/b3cc7b40-8751-43e9-8687-89b1b9465832)

I also navigated to the certificate itself and can see the following:

![Pasted image 20240422150422](https://github.com/lm3nitro/Projects/assets/55665256/76bb2a1a-3e1c-496a-89f4-78fdacec453e)

![Pasted image 20240422150539](https://github.com/lm3nitro/Projects/assets/55665256/d4e3ed86-bbbf-4a77-b289-70b02917b9f4)

The resilution for this is to have a proper SSL certificate. For the reasons I am using the ESXi server, this is not something that warrants immediate attention, but something that will be addressed later. 

### Summary:

In part 3 I was able to download, install and apply the needed patch in order to remediate the vulnerabilities previously discovered in Part 2. I then was able to validate that the server was no longer vulenrable to those CVEs and that it was indeed patched. Unpatched vulnerabilities can be exploited, which can lead to system compromises and other security incidents. Aside from the the CVEs, patching can often fix critical bugs that can affect the stability and integrity of the system. Its important to continually monitor and scan for vulnerabilities in order to maintain the overal health of the environment. 

Simply applying the patch is not enough. There also needs to verification to ensure thatt the patch was installed correctly and that it effectively addresses the vulnerabilities. Regualar vulnerability scanning and patch management promotes a proactive security posture. 
