# Splunk

<img width="486" alt="Screenshot 2024-04-27 at 2 10 57 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/962f67af-e61d-4ee9-98e2-cad078e3debb">

Splunk is utilized for conducting business and web analytics, managing applications, ensuring compliance, and enhancing security measures. Splunk is a SIEM (security information and event management) solution. It gathers data from logs, sensors, applications, and more, and organizes it so that users can perform searches to uncover insights, trends, and potential problems. It allows you to investigate and solve issues, monitor performance, and improve security across your IT infrastructure. 

### Scope:
I will be installing Splunk on my Ubuntu 22.04 VM in my home environment. I will also cover hot to install additional Add-Ons. Having add-ons will simplify the process of ingesting data from various sources (applications, servers, firewalls, etc.). I will then be sending a variety of logs to my Splunk instance, however, this will be covered as part of a separate project. I will start with the creation of my VM that will host Splunk and then take a deep dive into the installation of Splunk itself. 

### Tools and Technology:
Ubuntu and Splunk

## Create VM

For my environment, I decided to use Ubuntu 22.04 to install Splunk. To begin, I will set-up my VM and install Splunk. I am using VMWare Workstation 16 Pro and set the VM to the following parameters:

![Pasted image 20240328164129](https://github.com/lm3nitro/Projects/assets/55665256/774bffde-b410-41df-9cd9-b49beb1a3fc1)

![Pasted image 20240328164200](https://github.com/lm3nitro/Projects/assets/55665256/31fd05ba-429a-41b3-94af-28016d1585a7)

Here is the set-up for the hostname and configuring SSH during the initial install:

![Pasted image 20240328164820](https://github.com/lm3nitro/Projects/assets/55665256/4ed86501-61de-4f1c-8a3a-bd9f694bbc36)

![Pasted image 20240328170651](https://github.com/lm3nitro/Projects/assets/55665256/fb8ccf56-4dea-4e21-8d67-56af9ac31421)

##SSH via SecureCRT

Now that my Ubuntu VM is up and running, I then used SecureCRT to SSH into the server:

![Pasted image 20240328172017](https://github.com/lm3nitro/Projects/assets/55665256/4180bddb-898c-4a31-99f3-b20c3791a5ac)

## Install Splunk

To start of with my lab, I will install Splunk To begin the install, I went to verify the latest version. At the time of this writing it is version 9.2.1

![Pasted image 20240328164010](https://github.com/lm3nitro/Projects/assets/55665256/cfb65c66-c85c-4bb1-a35a-85b11e02bd5f)

I used the following command to download the latest version:

```
wget -O splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb "https://download.splunk.com/products/splunk/releases/9.2.1/linux/splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb"
```
Here we see that Splunk has been downloaded. Once downloaded, I proceed to run the following command:
```
dpkg -i splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb
```

![Pasted image 20240328172216](https://github.com/lm3nitro/Projects/assets/55665256/6d18b3bb-75a0-4170-8d5f-fefc87bca6da)

I then accepted the terms presented:

![Pasted image 20240328172414](https://github.com/lm3nitro/Projects/assets/55665256/6acafe66-49ad-4152-9c7e-25e8adb4c524)

During the install, we will get prompted with the username and password for Splunk:

![Pasted image 20240328172540](https://github.com/lm3nitro/Projects/assets/55665256/b6a837a4-b7ec-49f0-bada-f99fb88f932a)

Once the install is complete, we can see that Splunk can now be accessed via port 8000:

![Pasted image 20240328172630](https://github.com/lm3nitro/Projects/assets/55665256/5b83e5df-8805-4a82-8845-acfe68b31a96)

I also verified that all the logs were generated for Splunk:

![Pasted image 20240328172334](https://github.com/lm3nitro/Projects/assets/55665256/4de4e42a-517e-467b-bbf4-2fc6821a3f0d)

## Accessing Splunk

I then navigated to my Splunk via the web browser on port 8000 and was presented with the login screen:

![Pasted image 20240331163727](https://github.com/lm3nitro/Projects/assets/55665256/c00077b2-1112-4d8c-bd2e-20150db8941d)

I will go ahead and check the CPU and verify that everything is working properly.

Here is the the CPU when I initially installed Splunk. Everything looks good.

![Pasted image 20240331163921](https://github.com/lm3nitro/Projects/assets/55665256/6336f681-7a6f-4847-b96d-f29559a28753)

![Pasted image 20240331163943](https://github.com/lm3nitro/Projects/assets/55665256/d9db6d4d-e406-4534-806c-873a7ca71f57)

## Download and Install Add-Ons for Splunk

Now that I have my Splunk instance up and running, I then proceeded to install some add-ons. Add-ons extend Splunk's functionality and allows for integration with various data sources and technologies. In my lab, I will be collecting data from Zeek, Palo Alto, Suricata, and Squid Proxy.

![Pasted image 20240328174901](https://github.com/lm3nitro/Projects/assets/55665256/f6998ff3-8dd6-4997-835e-dd9bccba8cd9)

![Pasted image 20240328175008](https://github.com/lm3nitro/Projects/assets/55665256/b9655edc-a64f-4878-aaaa-86258f723959)

![Pasted image 20240328175040](https://github.com/lm3nitro/Projects/assets/55665256/9e119adc-8730-44e1-b744-5baddc674865)

![Pasted image 20240328174937](https://github.com/lm3nitro/Projects/assets/55665256/d8b13770-5550-406b-853b-3e73f2fda0bd)


I checked and verified the CPU as well to ensure efficient system performance:

![Pasted image 20240331153652](https://github.com/lm3nitro/Projects/assets/55665256/b77b171b-9ad3-47f0-8d9f-abdaf75ce3dc)

![Pasted image 20240331153506](https://github.com/lm3nitro/Projects/assets/55665256/93382723-6afa-4c29-96bb-6881d1b768b1)

![Pasted image 20240331153549](https://github.com/lm3nitro/Projects/assets/55665256/f516253e-725a-4fab-839f-0fd8fccfb12c)

I can also see the amount of events that had been sent to Splunk from the various sources:

![Pasted image 20240331153221](https://github.com/lm3nitro/Projects/assets/55665256/6f5a0928-7471-4ca2-840b-f3ed33af272f)

### Summary:

In this project I was able to install Splunk and also installed additional add-ons in order to better integrate data from various sources. Sending this data from various sources will be included as part of other projects. Installing Splunk also allowed me to gain hands on experience with its functionalities, including data ingestion, search, and visualization. I also want to create various dashboards and use the different features to better viausalize the inforamtion that is important to me and pertinent to my environment. I highly recommend having a SIEM as part of your network to. The following benefits can be gained from it:
+ Threat detection
+ Centralized Monitoring
+ Incident response
+ Visibility into Network Activity
+ Learning Environment

