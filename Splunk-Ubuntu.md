Ubuntu

![Pasted image 20240328164820](https://github.com/lm3nitro/Projects/assets/55665256/4ed86501-61de-4f1c-8a3a-bd9f694bbc36)


![Pasted image 20240328170651](https://github.com/lm3nitro/Projects/assets/55665256/fb8ccf56-4dea-4e21-8d67-56af9ac31421)

![Pasted image 20240328172017](https://github.com/lm3nitro/Projects/assets/55665256/4180bddb-898c-4a31-99f3-b20c3791a5ac)



![Pasted image 20240328172216](https://github.com/lm3nitro/Projects/assets/55665256/6d18b3bb-75a0-4170-8d5f-fefc87bca6da)


![Pasted image 20240328172334](https://github.com/lm3nitro/Projects/assets/55665256/4de4e42a-517e-467b-bbf4-2fc6821a3f0d)


![Pasted image 20240328172414](https://github.com/lm3nitro/Projects/assets/55665256/6acafe66-49ad-4152-9c7e-25e8adb4c524)



![Pasted image 20240328172540](https://github.com/lm3nitro/Projects/assets/55665256/b6a837a4-b7ec-49f0-bada-f99fb88f932a)


![Pasted image 20240328172630](https://github.com/lm3nitro/Projects/assets/55665256/5b83e5df-8805-4a82-8845-acfe68b31a96)

![Pasted image 20240331163727](https://github.com/lm3nitro/Projects/assets/55665256/c00077b2-1112-4d8c-bd2e-20150db8941d)

![Pasted image 20240328164200](https://github.com/lm3nitro/Projects/assets/55665256/31fd05ba-429a-41b3-94af-28016d1585a7)




![Pasted image 20240331163921](https://github.com/lm3nitro/Projects/assets/55665256/6336f681-7a6f-4847-b96d-f29559a28753)



![Pasted image 20240328164010](https://github.com/lm3nitro/Projects/assets/55665256/cfb65c66-c85c-4bb1-a35a-85b11e02bd5f)



![Pasted image 20240331163943](https://github.com/lm3nitro/Projects/assets/55665256/d9db6d4d-e406-4534-806c-873a7ca71f57)






wget -O splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb "https://download.splunk.com/products/splunk/releases/9.2.1/linux/splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb"
![Pasted image 20240328174901](https://github.com/lm3nitro/Projects/assets/55665256/f6998ff3-8dd6-4997-835e-dd9bccba8cd9)


![Pasted image 20240328175008](https://github.com/lm3nitro/Projects/assets/55665256/b9655edc-a64f-4878-aaaa-86258f723959)

![Pasted image 20240328175040](https://github.com/lm3nitro/Projects/assets/55665256/9e119adc-8730-44e1-b744-5baddc674865)


![Pasted image 20240328174937](https://github.com/lm3nitro/Projects/assets/55665256/d8b13770-5550-406b-853b-3e73f2fda0bd)

![Pasted image 20240328164129](https://github.com/lm3nitro/Projects/assets/55665256/774bffde-b410-41df-9cd9-b49beb1a3fc1)


zeeek install 


![Pasted image 20240328175144](https://github.com/lm3nitro/Projects/assets/55665256/a26b5a58-04a7-457a-8a49-3de6d9e06ab6)




dependesis :

 sudo apt-get install python3-git python3-semantic-version

![Pasted image 20240328180121](https://github.com/lm3nitro/Projects/assets/55665256/a2b8a55b-60bb-46c3-b27b-bee307b96302)


![Pasted image 20240328175953](https://github.com/lm3nitro/Projects/assets/55665256/f8e9ca90-36a5-488a-9448-b089fb0f5651)

 sudo apt-get install cmake make gcc g++ flex libfl-dev bison libpcap-dev libssl-dev python3 python3-dev swig zlib1g-dev

### Retrieving the Sources





git clone --recurse-submodules https://github.com/zeek/zeek



![Pasted image 20240328175850](https://github.com/lm3nitro/Projects/assets/55665256/4b3770f6-f7f7-4315-815c-30f530353c1f)


### Configuring and Building




./configure

![Pasted image 20240328180338](https://github.com/lm3nitro/Projects/assets/55665256/9907fe4a-dee7-4bf5-979f-59399e95430a)


make

![Pasted image 20240328180401](https://github.com/lm3nitro/Projects/assets/55665256/cbadbe06-10d0-4b3f-9858-84f79194f12a)


### Last command: 

make install


output:

![Pasted image 20240328193130](https://github.com/lm3nitro/Projects/assets/55665256/823c800a-a775-40ac-9f39-c62c2933639e)






![Pasted image 20240328193023](https://github.com/lm3nitro/Projects/assets/55665256/16d2314f-a2a5-45fd-b823-8ed79f36d6e6)


#######
Json format: 
option persistance jsoin format 

@load frameworks/files/extract-all-files  
redef ignore_checksums=T;  
redef LogAscii::use_json=T;



#####offloading:


for i in rx tx sg tso ufo gso gro lro; do ethtool -K enp0s8 $i off; done 

#######After zeek is installed, which is by default to the target directory /usr/local/zeek/bin. Execute the following command to add the _/opt/zeek/bin_ directory to the system **PATH** via _~/.bashrc_ file.

echo "export PATH=$PATH:/usr/local/zeek/bin" >> ~/.bashrc

######Next, reload the _~/.bashrc_ file and check the system **PATH** variable using the following command. You should see the _/opt/zeek/bin_ directory within the system PATH.


source ~/.bashrc  
echo $PATH



#### Read pcaps files  in zeek output in json format:

```
zeek -C -r ../tm1t.pcap LogAscii::use_json=T
```



no include:


More information about the about: 

https://github.com/zeek/zeek-docs/blob/master/log-formats.rst




include: 

Configuration of time zone:

timedatectl list-timezones

timedatectl set-timezone UTC





##### sending zeek logs to splunk 



zeek conn log json format:

![Pasted image 20240328212902](https://github.com/lm3nitro/Projects/assets/55665256/0584be89-ba59-4aec-ae76-69e872eed795)





zeek ja3 hash json format: 

![Pasted image 20240331164157](https://github.com/lm3nitro/Projects/assets/55665256/8188f8e7-9a8e-4814-8225-ccffaa01d00d)



###### Suricata:

Setup to install the latest stable Suricata from Personal Package Archives (PPA)

sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update

sudo apt-get install suricata


### Upgrading Suricata:

sudo apt-get update
sudo apt-get upgrade suricata





sending logs to splunk :


flow:

![Pasted image 20240331164810](https://github.com/lm3nitro/Projects/assets/55665256/0d0d8eba-1f96-4317-9f92-fd9af9c9248b)




alert :
![Pasted image 20240331164627](https://github.com/lm3nitro/Projects/assets/55665256/62b89c67-a98e-43e0-8c26-eed13331cfd9)


flow in detail json format: 
![Pasted image 20240331164454](https://github.com/lm3nitro/Projects/assets/55665256/c5f548c7-b219-4d84-b1a7-b0c95382bc78)











#### SPLUNK 8 BACKUP:



![Pasted image 20240331161322](https://github.com/lm3nitro/Projects/assets/55665256/d60a2e2a-7304-4c27-bee4-da7bba51e82d)


![Pasted image 20240331161734](https://github.com/lm3nitro/Projects/assets/55665256/70b88005-1b92-4e9d-b659-7e9c53a27d98)

![Pasted image 20240331161015](https://github.com/lm3nitro/Projects/assets/55665256/4fe525ae-cd0a-4017-8acb-92a0499e98f9)



![Pasted image 20240331160743](https://github.com/lm3nitro/Projects/assets/55665256/de9cb3dd-bcb1-4778-b1cc-307d3fb2bb69)


![Pasted image 20240331153812](https://github.com/lm3nitro/Projects/assets/55665256/c9c3ffb1-dadd-4f56-831e-0e5b5587d0f0)




![Pasted image 20240331152753](https://github.com/lm3nitro/Projects/assets/55665256/6d141fe7-4724-467a-bb56-93caa4ae97e2)



![Pasted image 20240331152325](https://github.com/lm3nitro/Projects/assets/55665256/8d1dcac5-1bd7-4564-af18-df9313c08093)



![Pasted image 20240331150634](https://github.com/lm3nitro/Projects/assets/55665256/508547e7-7cac-4a42-9b3b-155c43669283)


![Pasted image 20240331151611](https://github.com/lm3nitro/Projects/assets/55665256/cf3ac96f-6633-40fa-89f3-0df88350a48e)

![Pasted image 20240331153221](https://github.com/lm3nitro/Projects/assets/55665256/6f5a0928-7471-4ca2-840b-f3ed33af272f)


![Pasted image 20240331153652](https://github.com/lm3nitro/Projects/assets/55665256/b77b171b-9ad3-47f0-8d9f-abdaf75ce3dc)



![Pasted image 20240331153506](https://github.com/lm3nitro/Projects/assets/55665256/93382723-6afa-4c29-96bb-6881d1b768b1)


![Pasted image 20240331153549](https://github.com/lm3nitro/Projects/assets/55665256/f516253e-725a-4fab-839f-0fd8fccfb12c)



![Pasted image 20240331152232](https://github.com/lm3nitro/Projects/assets/55665256/674cf9fa-0ec1-4c2c-a9ce-08425280f80a)


###pfsense:


![Pasted image 20240331162214](https://github.com/lm3nitro/Projects/assets/55665256/8a3abddc-3d87-4557-aa06-f43a30f10a11)


![Pasted image 20240331161939](https://github.com/lm3nitro/Projects/assets/55665256/649aa069-e0a3-4928-a6d2-353c5a7da1d6)




![Pasted image 20240331155455](https://github.com/lm3nitro/Projects/assets/55665256/0b322e24-055c-46f1-8913-2bd2b0501425)


![Pasted image 20240331155547](https://github.com/lm3nitro/Projects/assets/55665256/6dd0ad15-9d96-4eb0-bf3a-7c06fc1c92d8)

![Pasted image 20240331155824](https://github.com/lm3nitro/Projects/assets/55665256/401e6649-223e-464b-a678-b680ff1e98b3)
![Pasted image 20240331155659](https://github.com/lm3nitro/Projects/assets/55665256/eb3dcda9-6d21-481d-b57a-4f0172e526c0)

![Pasted image 20240331155954](https://github.com/lm3nitro/Projects/assets/55665256/65d7fd56-6185-4f66-9799-38f1c10866c1)


![Pasted image 20240331151819](https://github.com/lm3nitro/Projects/assets/55665256/61aa89c4-c610-440c-b96b-f3a5e98483b6)

![Pasted image 20240331151902](https://github.com/lm3nitro/Projects/assets/55665256/a5ef5870-0fa3-4bbd-8bf5-4d215f358144)

