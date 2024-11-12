# $${\color{red}LibreNMS}$$

![Pasted image 20240515124908](https://github.com/lm3nitro/Projects/assets/55665256/239da2d4-1d5a-4920-90af-17c6acf92a3e)

LibreNMS is an open-source network monitoring and management platform used to monitor the health, performance, and availability of various network devices, services, and infrastructure components within an organization's network

These are some features that it offers:

+ Auto discovery
+ Alerting
+ Multiple environment sensors support
+ Multiple protocols data collection (STP, OSPF, BGP etc)
+ VLAN, ARP and FDB table collection
+ Customizable Dashboards
+ Device Backup integration (Oxidized, RANCID)
+ Multiple Authentication Methods (MySQL, LDAP, Active Directory, HTTP)
+ NetFlow, sFlow, IPFIX (NfSen)
+ Service monitoring (Nagios Plugins)
+ Syslog (Integrated, Graylog)
+ Traffic Billing (Quota, 95th Percentile)
+ Two Factor Authentication
+ API
+ Auto Updating


### Scope:
In this lab I will import the LibreNMS OVA into VMWare Workstation to install and configure LibreNMS and will configure my Fortinet firewall to send its SNMP traffic to LibreNMS so that I am able to monitor it. I will then capture this traffic and analyze it in Wireshark. 

### Tools and Technology
LibreNMS, VMWare Workstation, Fortinet, and Wireshark

### Network Diagram:

![Pasted image 20240515163239](https://github.com/lm3nitro/Projects/assets/55665256/f7d28476-0aa1-47d4-9055-bbebbb3afe20)

## Installing LibreNMS 

To get strated, I will need to download LibreNMS

![Pasted image 20240515125207](https://github.com/lm3nitro/Projects/assets/55665256/a2313ac2-d67e-49e6-981e-33b6dbaaf71d)

I will select to download an *OVA Images*:

![Pasted image 20240515125331](https://github.com/lm3nitro/Projects/assets/55665256/3d369bc5-45fe-4923-bd69-cc4bc2decbf5)

Selected the Ubuntu 22.04 VMware OVA:

![Pasted image 20240515125628](https://github.com/lm3nitro/Projects/assets/55665256/abed0708-629c-45cf-aaee-5d7d72f08a00)

Next, I will need to import the OVA file in VMware workstation

+ Select *Open a Virtual Machine*
+ Select the image previously downloaded
+ Click *Open*

![Pasted image 20240515125928](https://github.com/lm3nitro/Projects/assets/55665256/b5da1078-0293-4731-8d62-a922a338b2ba)

Select *Import*

![Pasted image 20240515130022](https://github.com/lm3nitro/Projects/assets/55665256/4447c319-aae0-4447-8457-8bf3fa747b6c)

Once the import was complete, prior to turn it on, I added more resources to the LibreNMS VM. I also selected the network adapter:

![Pasted image 20240515130236](https://github.com/lm3nitro/Projects/assets/55665256/d0c7dddc-ec6c-4c92-b2ec-82acc0d6ddd1)

## Access/Credentials:

If you are using the VirtualBox image then to access your newly imported VM, these ports are forwarded from your machine to the VM: 8080 for WebUI and 2023 for SSH. 

>#### Remember to edit/remove them if you change (as you should) the VM network configuration.

WebUI (http://lm3nitro.librenms.local)
username: librenms
password: D32fwefwef

SSH (change the password ssh://lm3nitro.librenms.local:2023)
username: librenms
password: CDne3fwdfds

SSH (remove this account)
username: vagrant
password; vagrant

MySQL/MariaDB
username: librenms
password: D42nf23rewD

I then logged in via SSH:

![Pasted image 20240515131644](https://github.com/lm3nitro/Projects/assets/55665256/838339ed-98f2-4485-8bbe-605254004953)

Verify the LibreNMS service status:

![Pasted image 20240515151928](https://github.com/lm3nitro/Projects/assets/55665256/8a3b8112-580b-44a9-8ef8-140b9cf7adb6)

I then proceeded to login into the WebUI:

lm3nitro.librenms.local

![Pasted image 20240515131844](https://github.com/lm3nitro/Projects/assets/55665256/49e00601-c53f-4688-98e8-c8a4f6fb78bf)

I also check the ngnix logs to see the loggin activity:

![Pasted image 20240515152113](https://github.com/lm3nitro/Projects/assets/55665256/4731dd9c-1878-4ee2-ad8e-c2828e2a72b9)

LibreNMS also allows you to check information about its own system. I took a look at some system information about the LibreNMS server. This can be checked under *Devices > All Devices > Server*

![Pasted image 20240515132257](https://github.com/lm3nitro/Projects/assets/55665256/1cb0bf2d-a702-4742-aa27-03168dc7e9cd)

Select *localhost*

![Pasted image 20240515132829](https://github.com/lm3nitro/Projects/assets/55665256/be74e20e-3a81-4117-9ffb-9cce97db7ccd)

These are the options the LibreNMS offers: 

![Pasted image 20240515133017](https://github.com/lm3nitro/Projects/assets/55665256/b61d079c-ddee-4c31-badd-0059e2ef22e5)

Graphs Tab:

![Pasted image 20240515133143](https://github.com/lm3nitro/Projects/assets/55665256/0c8fc4be-2c42-4bbf-be69-d68dd01bff52)

Health Tab:

![Pasted image 20240515133205](https://github.com/lm3nitro/Projects/assets/55665256/e434893d-9955-41af-b1e2-a93c11d49fbe)

Interfaces information:

![Pasted image 20240515133242](https://github.com/lm3nitro/Projects/assets/55665256/f881f272-b33a-43ff-b5c6-cdc1e017cdb6)

Being able to monitor the LibreNMS system itself is important becuase it ensures the health and performance of the monitoring infrastructure. Keeping track of the servers resources such as CPU, Memory,and Disk Space allows us identify and address potential issues affecting the systems reliability. 

## Adding Fortinet Firewall:

Now that I have LibreNMS up and running, I need to configure SNMP on my Fortinet Firewall. To do this, these are the steps I followed:

1. Navigiate to *Network > Interfaces* Select the management insterface and click *Edit* 

![Pasted image 20240515142002](https://github.com/lm3nitro/Projects/assets/55665256/9f6d2fa2-9d67-419a-ad49-38dc7c15b55f)

Once in the edit section I checked the SNMP box option:

![Pasted image 20240515142218](https://github.com/lm3nitro/Projects/assets/55665256/33d84a05-e790-4e01-a6ac-f11dce0d7d4c)

This will bring up a new dialog where the SNMP information will need to be entered (LibreNMS IP) :

![Pasted image 20240515142643](https://github.com/lm3nitro/Projects/assets/55665256/3e6d1f92-9808-4cb9-b959-9e4a49edfc77)

I also wanted to get a packet spture of the SNP traffc to ensure that everything was being sent and that it was configured correctly. To do this on Fortinet, navigate to *Network > Diagnostics > Packet Capture*. Then select the management interface where SNMP was configured, enter the IP, and start the capture:

![Pasted image 20240515141744](https://github.com/lm3nitro/Projects/assets/55665256/f632b758-0d4d-4bff-9029-6b6a3042f1a2)

Here is another view at the traffic once the packet capture was started:

![Pasted image 20240515141152](https://github.com/lm3nitro/Projects/assets/55665256/19843376-3968-4ff5-99e8-678e87c336ea)

After the SNMP is configured in Fortinet, I also need to add the Fortinet firewall in LibreNMS. Once logged into LibreNMS, navigate to *Devices > Add Device*

![Pasted image 20240515140841](https://github.com/lm3nitro/Projects/assets/55665256/739c3f3a-40af-4060-8293-a2c4dd6b2c38)

Added the Fortinet Firewall information:

![Pasted image 20240515140948](https://github.com/lm3nitro/Projects/assets/55665256/b7a59f8d-c7ac-4500-b987-910704aaa643)

I can see that it was successfully added:

![Pasted image 20240515141026](https://github.com/lm3nitro/Projects/assets/55665256/b20fd542-45d0-4a3a-9890-9eab339e0f19)

I went back to Fortinet to stop the capture:

![Pasted image 20240515141623](https://github.com/lm3nitro/Projects/assets/55665256/cee7c5d0-406a-4827-bcd5-5f432be6a38b)

I opened the pcap in Wireshark and verified the SNMP traffic:

![Pasted image 20240515141357](https://github.com/lm3nitro/Projects/assets/55665256/a274d505-ba87-4428-8552-687f6546f19f)

The Fortinet Management Interface was configured to send SNMP traffic to the LibreNMS server, I also added Fortinet to LibreNMS. I can now monitor Fortinet in LibreNMS.

## Fortinet SNMP Traffic

Here I can see the Fortinet I added previosly:

![Pasted image 20240515142952](https://github.com/lm3nitro/Projects/assets/55665256/d8b48a0c-76e3-4653-a42b-17ca8f536754)

Here I can see the the overview tab:

![Pasted image 20240515145024](https://github.com/lm3nitro/Projects/assets/55665256/5194f601-3d5b-4101-bdf3-28fc17e3cc1b)

Graphs TAB:

![Pasted image 20240515145153](https://github.com/lm3nitro/Projects/assets/55665256/cb87adbf-2c06-44cc-b2c0-d54e65add53c)

Heath information:

![Pasted image 20240515145259](https://github.com/lm3nitro/Projects/assets/55665256/7b17c465-aaa0-472e-9f83-45f14ae1ac1b)

Interfaces information: 

![Pasted image 20240515145434](https://github.com/lm3nitro/Projects/assets/55665256/5a0cef59-39a7-4d08-9e00-c8cf942dedf1)

Event logs information:

![Pasted image 20240515145837](https://github.com/lm3nitro/Projects/assets/55665256/9156c386-163a-4fce-9b2c-ad1a9a90ee65)

I can also see all the ARP information from Fortinet:

![Pasted image 20240515150818](https://github.com/lm3nitro/Projects/assets/55665256/b11229ec-e60f-41c1-bcca-8743c3e1d784)

LibreNSML also allows you to monitor interfaces in real time:

![Pasted image 20240515154612](https://github.com/lm3nitro/Projects/assets/55665256/5eb68f9d-1bab-49bf-a28d-63ae7091c43a)

## Configuring LibreNMS Pulling: to 60s:

One of the features that LibreNMS offers is support for polling data at intervals so that it can fit your needs:

>#### Be aware of the following:
> If you just want faster up/down alerts, Fast Ping is a much easier to get reach that goal. For this, you must also change your cron entry for poller-wrapper.py for this to work (if you change from the default 300 seconds). Your polling MUST complete in the time you configure for the heartbeat step value. See /poller in your WebUI for your current value. This will only affect RRD files created from the moment you change your settings. This change will affect all data storage mechanisms such as MySQL, RRD and InfluxDB. If you decrease the values, then be aware of the increase in space use for MySQL and InfluxDB. It's highly recommended to configure some performance optimizations. Keep in mind that all your devices will write all graphs every minute to the disk and that every device has many graphs. The most important thing is probably the RRDCached configuration that can save a lot of write IOPS.

To make the changes, navigate to /settings/poller/rrdtool/ within your WebUI. Select RRDTool Setup and then update the two values for step and heartbeat intervals. 

Step is how often you want to insert data, I will be changing this to 1 minute polling, so this should be 60 (seconds).

The 'Heartbeat' is how long to wait for data before registering a null value, i.e 120 seconds.

![Pasted image 20240515152539](https://github.com/lm3nitro/Projects/assets/55665256/5bad41c5-e3ad-4aa1-bb11-1c79fa40fedd)

The rest of the configuration

![Pasted image 20240515152651](https://github.com/lm3nitro/Projects/assets/55665256/491f5acd-ca19-4c24-9ff3-27aafc6b4910)

Once the changes have been made, the LibreNMS service will need to be restarted:

```
sudo systemctl restart librenms.service
```

![Pasted image 20240515152921](https://github.com/lm3nitro/Projects/assets/55665256/d4a34a26-29ed-4562-b2ac-9e1291c972c0)

After the service is restarted, I verified the last polled:

![Pasted image 20240515153216](https://github.com/lm3nitro/Projects/assets/55665256/84f8da29-e3f3-47be-b791-02c6bf3a8775)

The next steps is to convert the current rrdstep.php to reflect the changes made. For this, navigate to the LibreNMS directory where all the scripts are located:

![Pasted image 20240515155515](https://github.com/lm3nitro/Projects/assets/55665256/6dd90532-1a05-433b-abb6-30a6a6f853ac)

Convert existing RRD files:

![Pasted image 20240515160017](https://github.com/lm3nitro/Projects/assets/55665256/4297dce0-ef60-4f18-a58d-4eefe781bd9b)

Once complete, I can see the Poller information:

![Pasted image 20240515160130](https://github.com/lm3nitro/Projects/assets/55665256/c9181006-2175-4c71-aa45-31437cefa450)

I can also see the performance:

![Pasted image 20240515160206](https://github.com/lm3nitro/Projects/assets/55665256/9f704c2c-d923-412d-8e30-d5e410d4a2a0)


## Summary:

I was able to install and configure LibreNMS, configure Fortinet SNMP traffic and added the firewall to LibreNMS for monitoring. I also changed the configuration have polling done every 1 min. Having this configuration in my network allows me to track configuration changes, monitor performance, and identify any anomalies in resources or unusual traffic patterns. I enjoyed getting this setup and looking at its features and functionality. Although only a few were covered, LibreNMS offers many more features and is adaptable to meet your needs. 

