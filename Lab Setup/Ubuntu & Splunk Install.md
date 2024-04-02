## Install Ubuntu 22.04

For my lab, I decided to use Ubuntu 22.04 to install Splunk, Zeek, and Suricata. To begin, I am using VMWare Workstation 16 Pro and set the VM to the following perameters:

![Pasted image 20240328164129](https://github.com/lm3nitro/Projects/assets/55665256/774bffde-b410-41df-9cd9-b49beb1a3fc1)

![Pasted image 20240328164200](https://github.com/lm3nitro/Projects/assets/55665256/31fd05ba-429a-41b3-94af-28016d1585a7)

Here is the set-up for the hostname and configuring SSH during the initial install:

![Pasted image 20240328164820](https://github.com/lm3nitro/Projects/assets/55665256/4ed86501-61de-4f1c-8a3a-bd9f694bbc36)

![Pasted image 20240328170651](https://github.com/lm3nitro/Projects/assets/55665256/fb8ccf56-4dea-4e21-8d67-56af9ac31421)

## SSH via SecureCRT
Now that my Ubuntu VM is up and running, I then used SecureCRT to SSH into the server:

![Pasted image 20240328172017](https://github.com/lm3nitro/Projects/assets/55665256/4180bddb-898c-4a31-99f3-b20c3791a5ac)

## Install Splunk

To start of with my lab, I will install Splunk To begin the install, I went to verify the latest version:

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

I then navigated to my Splunk via the web browser on port 8000 and was presented with the login screen:

![Pasted image 20240331163727](https://github.com/lm3nitro/Projects/assets/55665256/c00077b2-1112-4d8c-bd2e-20150db8941d)

I will go ahead and check the CPU and verify that everything is working properly.

## Splunk CPU

Here is the the CPU when I initially installed Splunk. Everything is good.
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


