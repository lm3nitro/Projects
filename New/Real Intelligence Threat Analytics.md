
# Installation:



### RITA (Real Intelligence Threat Analytics)





![Pasted image 20240914110553](https://github.com/user-attachments/assets/3c450f17-2b6d-4519-8afa-533061e903b0)

### Compatibility :



![Pasted image 20240914115025](https://github.com/user-attachments/assets/9e35e0f0-2fbf-43a9-b159-50c8388426d9)


### System description:



![Pasted image 20240914115133](https://github.com/user-attachments/assets/30d1503c-2faf-4be9-99ce-f7fcf7eedb7b)


Go to the following link to download the latest release of the code:

https://github.com/activecm/rita/releases


![Pasted image 20240914114925](https://github.com/user-attachments/assets/f6e1751d-6739-40eb-a5b6-5d47ecee8ebc)


### Downloading the installation script:


![Pasted image 20240914113623](https://github.com/user-attachments/assets/bd8e362f-8ecd-4c3e-a4d8-9b80f629de72)


### Changing the permissions:


![Pasted image 20240914113709](https://github.com/user-attachments/assets/a63794a4-9d8d-4a06-9f6d-a3d270fc16be)



### Executing the installation script:


sudo ./install-rita-zeek-here.sh


![Pasted image 20240914113822](https://github.com/user-attachments/assets/df1bac69-01c6-422e-bd3f-2d2d8248f5f8)



Adding the Root password:


![Pasted image 20240914114026](https://github.com/user-attachments/assets/9742dadc-cf6e-4282-903d-38093e431bcb)



### Snip of the installation process.......


![Pasted image 20240914114218](https://github.com/user-attachments/assets/4e78a8e7-bac3-4340-9bc5-56ce582cdc3c)


### Selecting the Network interface:



![Pasted image 20240914114135](https://github.com/user-attachments/assets/73392058-6e0d-4832-9ef7-985e867f5eb3)


Using the arrows to go down and space to select:


![Pasted image 20240914114333](https://github.com/user-attachments/assets/b3dca26c-097c-42af-95c1-7822571fcafe)




![Pasted image 20240914114401](https://github.com/user-attachments/assets/423d3989-cb69-4d84-a4fb-091bd47a2bf2)




### Verify the docker version:

docker -v



![Pasted image 20240914115613](https://github.com/user-attachments/assets/a29538e0-bae7-4f3c-99bf-0c178f8f44ad)


### Verify the docker images:

 sudo docker images
 

![Pasted image 20240914114645](https://github.com/user-attachments/assets/378f0b68-d7d1-4d9f-9c45-79f03a4db4b4)



### Verify the status of the containers:

sudo docker ps


![Pasted image 20240914114721](https://github.com/user-attachments/assets/6189d652-7b36-4174-84af-09bf20b21b7a)



# Working with PCAPS:


 Identifying and detecting network beaconing behaviour using RITA to locate network threats and C2/beacon:


### Getting some statistics of the trace file:

capinfos zeus_24hr.pcap


![Pasted image 20240914120933](https://github.com/user-attachments/assets/cbdc4242-aff2-442c-afd2-9bf422476cad)


### Converting pcap in Zeek logs:



zeek readpcap ~/Documents/pcaps/zeus_24hr.pcap ~/Documents/zeek-logs/


![Pasted image 20240914121409](https://github.com/user-attachments/assets/b02f1acf-e6d7-4dfe-a598-6a117a869116)



### Importing zeek logs to Rita:



Getting help using Rita:

rita -h 


![Pasted image 20240914122601](https://github.com/user-attachments/assets/c47e757b-0ab2-4b40-bcb3-35e32db9c8be)


rita import -l zeek-logs/ -d hunting


![Pasted image 20240914122742](https://github.com/user-attachments/assets/079d86f2-2256-45a6-a93d-23afb48ac70e)





### Listing the Database:


![Pasted image 20240915131312](https://github.com/user-attachments/assets/2dda5a68-6d9e-4743-b794-2f139e195ad9)


### View the Database:

rita view hunting


![Pasted image 20240914123316](https://github.com/user-attachments/assets/99aca490-17a9-45ab-b5a1-83335002d6f6)




![Pasted image 20240914123303](https://github.com/user-attachments/assets/7d09fd46-c433-452a-8065-2cde4b8f5202)



### Investigation of the FQDN with virustotal:


![Pasted image 20240914123526](https://github.com/user-attachments/assets/0ff56d64-f804-4949-a363-0d19b4d55e6f)


### Reading Zeek logs with Zeekcutter utility:

Downloading and installation of zeekcutter


mkdir /home/lm3nitro/Documents
cd /home/lm3nitro/Documents
wget https://raw.githubusercontent.com/activecm/zcutter/main/zcutter.py -O zcutter.py


![Pasted image 20240914125044](https://github.com/user-attachments/assets/4af2479e-6ca7-4cf0-8e44-fa80d3882630)

Changing permissions:

chmod 755 zcutter.py
if ! type zeek-cut >/dev/null 2>&1 ; then ln -s zcutter.py zeek-cut ; fi


![Pasted image 20240914125229](https://github.com/user-attachments/assets/a6ca3f96-33b1-4644-ba4f-a1e02f2afa53)



## Searching for the FQDN on http.logs:

I'm goanna be looking on the http.logs file since Rita flag the FQDN mahamaya1ifesciences.com user-agent: mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; SV1) 

cat http.log | ~/Documents/tools/zcutter.py | head


![Pasted image 20240914130859](https://github.com/user-attachments/assets/2323a41b-14fb-4ca0-8a52-1467504c7943)



 The domain mahamaya1ifesciences.com is mapped to the IP 67.207.93.135, it seems like the internal IP downloaded a image file from the server at  /metro91/admin/1/ppptp.jpg



### Looking at the conversation with Wireshark:

It seems that the internal host at 192.168.99.53 was in constant communication with the external IP 67.207.93.135 24 hours:



![Pasted image 20240914132522](https://github.com/user-attachments/assets/39f6c072-7092-4659-845b-7b2274084dcf)


### Payload inspection and extraction using Wireshark 

It seems that the image is encoded in base64 and compressed in Gzip, also there is some PowerShell code attach to the payload to be able to run in memory. 


![Pasted image 20240914124705](https://github.com/user-attachments/assets/d58e18f1-32b1-46dc-a166-1aecd502970a)


### Decoding the payload using Cyberchef:




![Pasted image 20240914124448](https://github.com/user-attachments/assets/ce10519b-f298-4da3-9dce-7896b7ecd996)





Malware: Zeus
Connection Type: Reverse HTTP
C2 Platform: Cobalt Strike
Host Payload Delivery Method: Powershell one-liner
Target Host/Victim: 192.168.99.53
C2 Server: 67.207.93.135 
