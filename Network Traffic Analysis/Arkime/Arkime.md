# $${\color{aqua}Arkime}$$

![Pasted image 20240512213410](https://github.com/lm3nitro/Projects/assets/55665256/9a463061-5b9b-4bc7-84e2-c088b15a83ad)

Arkime (formerly known as Moloch) is an open-source large-scale network traffic analysis tool that is designed to capture, index, and analyze network packets. It enables security analysts to conduct real-time, high-speed searches and data visualizations, aiding in the detection, diagnosis, and response to cybersecurity threats and anomalies within network environments.

### Scope:
I will be installing and configuring Arkime and will be analyzing coming and going traffic from my internal servers.  

### Tools and Technology:
Arkime, Elasticsearch, Wireshark and Linux OS

### Network Diagram:

Here is a diagram of how I have everything set-up for the this project:

![Pasted image 20240513172414](https://github.com/lm3nitro/Projects/assets/55665256/b661ef4f-0544-421b-b12f-fb9864816fda)

## Installation
To get started, I navigated to the official Arkime website and then navigated to *Downloads*

https://arkime.com/

![Pasted image 20240512190635](https://github.com/lm3nitro/Projects/assets/55665256/01d8f984-7f02-4103-a7a3-72e62e53d726)

Before downloading, I needed to verify the Ubuntu version I was running:

```
lsb_release -a
```

![Pasted image 20240512190658](https://github.com/lm3nitro/Projects/assets/55665256/6a9ebfeb-6345-43f8-87af-ae2169c4e9e7)

Included are the basic Arkime Installation steps:

![Pasted image 20240512214155](https://github.com/lm3nitro/Projects/assets/55665256/809f6be2-af83-4978-b289-e4c3bab9ddd6)

Next, I needed to locate the version that is compatible with my OS/Kernel:

![Pasted image 20240512190821](https://github.com/lm3nitro/Projects/assets/55665256/e6fa1377-dacc-46b6-8a1a-e0b288a5c816)

Right-click on the package and select *Copy Link*

![Pasted image 20240512190944](https://github.com/lm3nitro/Projects/assets/55665256/b2da40bb-61e2-46c5-85ca-6f6ba1b33c2a)

Once you have the link, you can use wget and paste it into the command as seen below:

![Pasted image 20240512191029](https://github.com/lm3nitro/Projects/assets/55665256/7cb1236d-8ac0-42fd-b166-03b9e135c622)

Next, I installed the dependencies:

![Pasted image 20240512191135](https://github.com/lm3nitro/Projects/assets/55665256/59245fe6-1554-45f8-80e6-a6ba7ad5fe06)

Once the dependencies were installed, I unpackage Arkime:

![Pasted image 20240512191409](https://github.com/lm3nitro/Projects/assets/55665256/f4736183-4b8a-47b1-9c85-90cfaa87ac69)

Next, I need to verify the name of the ethernet interface I will be using:

![Pasted image 20240512191600](https://github.com/lm3nitro/Projects/assets/55665256/425053d5-e17e-4c69-9046-af915baeb725)

Setting up the sniffing interface and installing elasticsearch:

![Pasted image 20240512192000](https://github.com/lm3nitro/Projects/assets/55665256/7846e62d-e2de-4428-ab64-c19244c4aeb8)

Starting Elasticsearch:

![Pasted image 20240512192439](https://github.com/lm3nitro/Projects/assets/55665256/a08f7572-4599-434f-b569-ee0666047cc6)

I also verified the listening ports:

```
sudo ss -tnlp
```

![Pasted image 20240512192535](https://github.com/lm3nitro/Projects/assets/55665256/70c464b6-fd5d-4608-a257-76784ade98b1)


Initialization of Elasticsearch one more time:

```
sudo /opt/arkime/db/db.pl http://lm3nitro.arkime.local:9200 init
```

![Pasted image 20240512192700](https://github.com/lm3nitro/Projects/assets/55665256/4604de42-f2b6-48e9-b7c9-7e341232c5c6)

Now that it's installed, I created a user lm3nitro:

```
sudo /opt/arkime/bin/arkime_add_user.sh admin "Admin User" lm3nitro --admin
```

![Pasted image 20240512192848](https://github.com/lm3nitro/Projects/assets/55665256/3cdb38b7-84eb-4ab2-a49a-b72bfd7ccd41)


After creating the users, lets starts the services:

```
sudo systemctl start arkimecapture.service
sudo systemctl start arkimeviewer.service
```
![Pasted image 20240512193932](https://github.com/lm3nitro/Projects/assets/55665256/562ebd03-7257-4743-a4ea-55d0f3384bcf)

Verify the status of the services:

```
systemctl status arkimeviewer.service
```

![Pasted image 20240512193838](https://github.com/lm3nitro/Projects/assets/55665256/542e4030-c575-44bb-afb3-4bf0bc968ea9)

```
systemctl status arkimecapture.service
```

![Pasted image 20240512194344](https://github.com/lm3nitro/Projects/assets/55665256/31017185-dfa6-49cb-a99e-85b3708b8e2b)

Having the service started will generate logs. I took look at them below:

>#### Note: All logs are located in /opt/arkime/logs/
![Pasted image 20240512213310](https://github.com/lm3nitro/Projects/assets/55665256/42b99320-6c23-435a-836a-c0de8774f31c)

logs from viewer.log

```
sudo tail /opt/arkime/logs/viewer.log
```
![Pasted image 20240512213207](https://github.com/lm3nitro/Projects/assets/55665256/e3954332-1972-4028-b9fd-7abee832ea31)

logs from capture.log

```
sudo tail /optarkime/logs/capture.log
```

![Pasted image 20240512213034](https://github.com/lm3nitro/Projects/assets/55665256/a458b52c-e4e6-47d5-a665-1ff38227eacd)


## Troubleshooting
The following are some troubleshooting commands:

```
curl http://localhost:9200/_cat/health
```

![Pasted image 20240512201217](https://github.com/lm3nitro/Projects/assets/55665256/7e716bdc-73a3-447a-9316-90b1e889ce17)

```
curl http://localhost:9200/_refresh
```

![Pasted image 20240512201452](https://github.com/lm3nitro/Projects/assets/55665256/aeb91167-4116-4baf-a40d-cc37d7f9cfc0)

```
opt/arkime/db/db.pl http://127.0.0.1:9200 info
```

![Pasted image 20240512201539](https://github.com/lm3nitro/Projects/assets/55665256/409f2c8e-8507-4da9-9fd4-9b6a4887f024)

```
sudo /opt/arkime/bin/arkime_update_geo.sh
```

![Pasted image 20240512212835](https://github.com/lm3nitro/Projects/assets/55665256/aaf726fc-bba2-41ca-92b0-4152bf9f2c65)


## Logging into Arkime

Once everything is installed and running, I logged in:

![Pasted image 20240512211851](https://github.com/lm3nitro/Projects/assets/55665256/47c65a4e-1e8f-4e34-8e92-31cfd75135ea)

Once logged in, I took a look aroung. This is the connections tab:

![Pasted image 20240512203419](https://github.com/lm3nitro/Projects/assets/55665256/84073b51-446f-4a49-b42e-777d40079e3b)

SPIView
![Pasted image 20240512203128](https://github.com/lm3nitro/Projects/assets/55665256/985be448-9787-43ac-a9a0-4d2495c65818)

On my internal host I navigated to chess.com to see if I could capture the traffic and view it:

![Pasted image 20240512204654](https://github.com/lm3nitro/Projects/assets/55665256/7714e9af-0281-4ae2-a7f9-e45519a0dcb3)

Arkime also allows you to download pcap files:

![Pasted image 20240512204055](https://github.com/lm3nitro/Projects/assets/55665256/21c3881c-04b6-4839-81d0-4b98b9a5753e)

Lets open the pcap with Wireshark:

![Pasted image 20240512204851](https://github.com/lm3nitro/Projects/assets/55665256/a080dc0c-28e1-4042-9b84-169143447daa)

Looking at some DNS traffic:

![Pasted image 20240512205707](https://github.com/lm3nitro/Projects/assets/55665256/ba405fe3-9eb4-4d00-98ac-620d2320126e)

Arkime statistics:

![Pasted image 20240512210029](https://github.com/lm3nitro/Projects/assets/55665256/17838492-8ec9-4eed-b370-24396a5d4d20)

Looking at 1 hour worth of traffic: 

![Pasted image 20240512214512](https://github.com/lm3nitro/Projects/assets/55665256/1dd922b3-ed6e-4ace-8808-420b333fc1cd)


## Summary:

WI installed and configured Arkime and tested its functionality of capturing network traffic. Arkime is especially valuable becauseit gives you a searchable database of network traffic that makes it easy to search, investigate, and analyze network events. As seen in the screenshots above, its web interface offers an excellent way to filter for traffic, see connections, and explore network connections which is crucial for troubleshooting network issues and threat hunting. 


