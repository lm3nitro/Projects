## Install Ubuntu 22.04

For my lab, I decided to use Ubuntu 22.04 to install Splunk, Zeek, and Suricata. To begin, I used VMWare Workstation and set to following perameters:

![Pasted image 20240328164129](https://github.com/lm3nitro/Projects/assets/55665256/774bffde-b410-41df-9cd9-b49beb1a3fc1)

![Pasted image 20240328164200](https://github.com/lm3nitro/Projects/assets/55665256/31fd05ba-429a-41b3-94af-28016d1585a7)

Here is the set-up for the hostname and configuring SSH during the initial install:

![Pasted image 20240328164820](https://github.com/lm3nitro/Projects/assets/55665256/4ed86501-61de-4f1c-8a3a-bd9f694bbc36)

![Pasted image 20240328170651](https://github.com/lm3nitro/Projects/assets/55665256/fb8ccf56-4dea-4e21-8d67-56af9ac31421)

## SSH via SecureCRT
Now that my VM is up and Ubuntu is set-up, I then used SecureCRT to SSH into the server:

![Pasted image 20240328172017](https://github.com/lm3nitro/Projects/assets/55665256/4180bddb-898c-4a31-99f3-b20c3791a5ac)

## Install Splunk

Now I need to install Splunk. To begin with the install, I went to splunk to download the latest version:

![Pasted image 20240328164010](https://github.com/lm3nitro/Projects/assets/55665256/cfb65c66-c85c-4bb1-a35a-85b11e02bd5f)

```
wget -O splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb "https://download.splunk.com/products/splunk/releases/9.2.1/linux/splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb"
```
Here we see that Splunk has been installed:
![Pasted image 20240328172216](https://github.com/lm3nitro/Projects/assets/55665256/6d18b3bb-75a0-4170-8d5f-fefc87bca6da)

![Pasted image 20240328172414](https://github.com/lm3nitro/Projects/assets/55665256/6acafe66-49ad-4152-9c7e-25e8adb4c524)

During the install, we will get prompted with the username and password for Splunk:

![Pasted image 20240328172540](https://github.com/lm3nitro/Projects/assets/55665256/b6a837a4-b7ec-49f0-bada-f99fb88f932a)

Once the install is complete, we can see that Splunk can now be accessed via port 8000:

![Pasted image 20240328172630](https://github.com/lm3nitro/Projects/assets/55665256/5b83e5df-8805-4a82-8845-acfe68b31a96)

I also verified that all the logs were generated for Splunk:

![Pasted image 20240328172334](https://github.com/lm3nitro/Projects/assets/55665256/4de4e42a-517e-467b-bbf4-2fc6821a3f0d)

I then navigated to my Splunk via web browser on port 8000 and was presented with the login screen:

![Pasted image 20240331163727](https://github.com/lm3nitro/Projects/assets/55665256/c00077b2-1112-4d8c-bd2e-20150db8941d)

Now that Splunk is installed, I wanted to login and check the CPU and verify that everything is working properly.

## Splunk CPU

Here is the the CPU whe I initially installed Splunk. Everything looks good.
![Pasted image 20240331163921](https://github.com/lm3nitro/Projects/assets/55665256/6336f681-7a6f-4847-b96d-f29559a28753)

![Pasted image 20240331163943](https://github.com/lm3nitro/Projects/assets/55665256/d9db6d4d-e406-4534-806c-873a7ca71f57)

## Download and Install Add-Ons for Splunk

Now that we have our Splunk up and running, we will then need to install some add-ons. Add-ons extend Splunk's functionality and allows for integration with various data sources and technologies. In my lab, I will be collecting data from Zeek, Palo Alto, Suricata, and Squid Proxy.
![Pasted image 20240328174901](https://github.com/lm3nitro/Projects/assets/55665256/f6998ff3-8dd6-4997-835e-dd9bccba8cd9)

![Pasted image 20240328175008](https://github.com/lm3nitro/Projects/assets/55665256/b9655edc-a64f-4878-aaaa-86258f723959)

![Pasted image 20240328175040](https://github.com/lm3nitro/Projects/assets/55665256/9e119adc-8730-44e1-b744-5baddc674865)

![Pasted image 20240328174937](https://github.com/lm3nitro/Projects/assets/55665256/d8b13770-5550-406b-853b-3e73f2fda0bd)

## Zeek Install

The next part is the installation of Zeek. Before we can install Zeek, we need to ensure that we have the required dependencies.
![Pasted image 20240328175144](https://github.com/lm3nitro/Projects/assets/55665256/a26b5a58-04a7-457a-8a49-3de6d9e06ab6)

>##### Install Zeek Dependencies 
```
sudo apt-get install python3-git python3-semantic-version
```
![Pasted image 20240328180121](https://github.com/lm3nitro/Projects/assets/55665256/a2b8a55b-60bb-46c3-b27b-bee307b96302)

```
sudo apt-get install cmake make gcc g++ flex libfl-dev bison libpcap-dev libssl-dev python3 python3-dev swig zlib1g-dev
```
![Pasted image 20240328175953](https://github.com/lm3nitro/Projects/assets/55665256/f8e9ca90-36a5-488a-9448-b089fb0f5651)

>##### Retrieving the Sources
```
git clone --recurse-submodules https://github.com/zeek/zeek
```
![Pasted image 20240328175850](https://github.com/lm3nitro/Projects/assets/55665256/4b3770f6-f7f7-4315-815c-30f530353c1f)

>##### Configuring and Building
```
./configure
```
![Pasted image 20240328180338](https://github.com/lm3nitro/Projects/assets/55665256/9907fe4a-dee7-4bf5-979f-59399e95430a)

After configuring, run the make command to build Zeek:
```
make
```
![Pasted image 20240328180401](https://github.com/lm3nitro/Projects/assets/55665256/cbadbe06-10d0-4b3f-9858-84f79194f12a)

Lastly, we use the 'make install' to install Zeek to the specified prefix directory (in this case, /usr/local/zeek):
```
make install
```
Output:

![Pasted image 20240328193130](https://github.com/lm3nitro/Projects/assets/55665256/823c800a-a775-40ac-9f39-c62c2933639e)

Once that is complete, we can use the the following command to verify the version of Zeek that is installed:
```
./zeek -h
```
![Pasted image 20240328193023](https://github.com/lm3nitro/Projects/assets/55665256/16d2314f-a2a5-45fd-b823-8ed79f36d6e6)

The following modifications to Zeek's configuration are optional:

>##### Change Zeek logs to output in JSON format persistently, you can use Zeek's built-in functionality to output logs in JSON format directly.

Edit zeekctl.cfg and add the following:
```
@load frameworks/files/extract-all-files  
redef ignore_checksums=T;  
redef LogAscii::use_json=T;
```
For offloading:
```
for i in rx tx sg tso ufo gso gro lro; do
    ethtool -K enp0s8 $i off
done
```
After zeek is installed, which is by default to the target directory /usr/local/zeek/bin. Execute the following command to add the _/opt/zeek/bin_ directory to the system **PATH** via _~/.bashrc_ file.
```
echo "export PATH=$PATH:/usr/local/zeek/bin" >> ~/.bashrc
```
Next, reload the _~/.bashrc_ file and check the system **PATH** variable using the following command. You should see the _/opt/zeek/bin_ directory within the system PATH.
```
source ~/.bashrc  
echo $PATH
```

To read pcaps files with zeek and have the output in json format:

```
zeek -C -r ../tm1t.pcap LogAscii::use_json=T
```
More information about Zeek log formats can be found in the link below:

https://github.com/zeek/zeek-docs/blob/master/log-formats.rst

Setting the correct time zone in Zeek is important for accurate log timestamps and facilitating correlation with other logs.

>##### Configuration of time zone:
```
timedatectl list-timezones
```
```
timedatectl set-timezone UTC
```

We can now see how the zeek conn.log in json format in Splunk:

![Pasted image 20240328212902](https://github.com/lm3nitro/Projects/assets/55665256/0584be89-ba59-4aec-ae76-69e872eed795)

Here we can also see the ja3 hashes json format: 

![Pasted image 20240331164157](https://github.com/lm3nitro/Projects/assets/55665256/8188f8e7-9a8e-4814-8225-ccffaa01d00d)

## Install Suricata

Next, I will install the latest stable version of Suricata from Personal Package Archives (PPA)
```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata
```

>##### Upgrade Suricata:
```
sudo apt-get update
sudo apt-get upgrade suricata
```
Once I installed and upgraded suricata, I then began to send logs to Splunk.

Flow:

![Pasted image 20240331164810](https://github.com/lm3nitro/Projects/assets/55665256/0d0d8eba-1f96-4317-9f92-fd9af9c9248b)

Alerts:

![Pasted image 20240331164627](https://github.com/lm3nitro/Projects/assets/55665256/62b89c67-a98e-43e0-8c26-eed13331cfd9)

Flow in detailed json format: 

![Pasted image 20240331164454](https://github.com/lm3nitro/Projects/assets/55665256/c5f548c7-b219-4d84-b1a7-b0c95382bc78)


## Palo Alto

Now that I have Splunk installed and it is receiving traffic from Zeek and Suricata, I now want to also send my Firewall logs to Splunk as well. Here is the initial login page:

![Pasted image 20240331161322](https://github.com/lm3nitro/Projects/assets/55665256/d60a2e2a-7304-4c27-bee4-da7bba51e82d)

Here I can see the traffic form my network:

![Pasted image 20240331161734](https://github.com/lm3nitro/Projects/assets/55665256/70b88005-1b92-4e9d-b659-7e9c53a27d98)

The dashboard:

![Pasted image 20240331161015](https://github.com/lm3nitro/Projects/assets/55665256/4fe525ae-cd0a-4017-8acb-92a0499e98f9)

General Info:

![Pasted image 20240331160743](https://github.com/lm3nitro/Projects/assets/55665256/de9cb3dd-bcb1-4778-b1cc-307d3fb2bb69)

I Splunk I set up 2 seperate indexes for each of my Firewalls (Palo Alto and PfSense)

![Pasted image 20240331153812](https://github.com/lm3nitro/Projects/assets/55665256/c9c3ffb1-dadd-4f56-831e-0e5b5587d0f0)

I also verified that I was indeed getting logs to Splunk:

![Pasted image 20240331152753](https://github.com/lm3nitro/Projects/assets/55665256/6d141fe7-4724-467a-bb56-93caa4ae97e2)

![Pasted image 20240331152325](https://github.com/lm3nitro/Projects/assets/55665256/8d1dcac5-1bd7-4564-af18-df9313c08093)

Also, I can see that my indexes are getting information:

![Pasted image 20240331150634](https://github.com/lm3nitro/Projects/assets/55665256/508547e7-7cac-4a42-9b3b-155c43669283)

Here I can see that I am getting application detection form the NGFW. 

![Pasted image 20240331151611](https://github.com/lm3nitro/Projects/assets/55665256/cf3ac96f-6633-40fa-89f3-0df88350a48e)

Application detection information from a Next-Generation Firewall (NGFW) is crucial for several reasons:

- Granular Control
- Security Policy Enforcement
- Threat Detection
- Visibility and Analytics
- Compliance and Reporting
  
I can also see the amount of events that have been generated in Splunk:

![Pasted image 20240331153221](https://github.com/lm3nitro/Projects/assets/55665256/6f5a0928-7471-4ca2-840b-f3ed33af272f)

I checked and verified the CPU as well to ensure efficient system performance:

![Pasted image 20240331153652](https://github.com/lm3nitro/Projects/assets/55665256/b77b171b-9ad3-47f0-8d9f-abdaf75ce3dc)

![Pasted image 20240331153506](https://github.com/lm3nitro/Projects/assets/55665256/93382723-6afa-4c29-96bb-6881d1b768b1)

![Pasted image 20240331153549](https://github.com/lm3nitro/Projects/assets/55665256/f516253e-725a-4fab-839f-0fd8fccfb12c)


###pfsense:

Login:

![Pasted image 20240331162214](https://github.com/lm3nitro/Projects/assets/55665256/8a3abddc-3d87-4557-aa06-f43a30f10a11)

System information:

![Pasted image 20240331161939](https://github.com/lm3nitro/Projects/assets/55665256/649aa069-e0a3-4928-a6d2-353c5a7da1d6)

Dashboard:

![Pasted image 20240331155455](https://github.com/lm3nitro/Projects/assets/55665256/0b322e24-055c-46f1-8913-2bd2b0501425)

Rules:

![Pasted image 20240331155547](https://github.com/lm3nitro/Projects/assets/55665256/6dd0ad15-9d96-4eb0-bf3a-7c06fc1c92d8)

Nat:

![Pasted image 20240331155824](https://github.com/lm3nitro/Projects/assets/55665256/401e6649-223e-464b-a678-b680ff1e98b3)

Rules:

![Pasted image 20240331155659](https://github.com/lm3nitro/Projects/assets/55665256/eb3dcda9-6d21-481d-b57a-4f0172e526c0)

Logs:

![Pasted image 20240331155954](https://github.com/lm3nitro/Projects/assets/55665256/65d7fd56-6185-4f66-9799-38f1c10866c1)

Splunk info:

![Pasted image 20240331151819](https://github.com/lm3nitro/Projects/assets/55665256/61aa89c4-c610-440c-b96b-f3a5e98483b6)

PfSense fields in Splunk:

![Pasted image 20240331151902](https://github.com/lm3nitro/Projects/assets/55665256/a5ef5870-0fa3-4bbd-8bf5-4d215f358144)

