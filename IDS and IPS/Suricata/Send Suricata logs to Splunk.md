
# Sending Suricata Logs to Splunk

After deploying Suricata, I wanted to integrate its logs to my Splunk Instance. In order to do this, I needed to install the Splunk Fowarder in my server where Suricata was instralled. By default the listening port for the Splunk Fowarder is port 9997. I decided to use this port on my Splunk instance to listen for incoming communications from the forwarder. First I will set this listening port in Splunk and will then move to the Uuntu Server where the fowarder will be installed. 

## Configure a Receiving Port in Splunk

I used the Splunk Web GUI to configure a receiver:

1. Log into Splunk Web as a user with the admin role.
    
2. In Splunk Web, go to **Settings > Forwarding and receiving**.
    
3. Select "Configure receiving."
    
4. Verify if there are existing receiver ports open. You cannot create a duplicate receiver port. The conventional receiver port configured on indexers is port `9997`.
    
![Pasted image 20240415160852](https://github.com/lm3nitro/Projects/assets/55665256/54b9d825-c267-4a82-9793-2358273f544c)

5. Select "New Receiving Port."
    
6. Add a port number and save. I went with the default 9997.

![Pasted image 20240415161035](https://github.com/lm3nitro/Projects/assets/55665256/b90bd2a7-d637-4f2d-b972-8a1707a5bbef)

Next, I created an index where my Suricata Logs will be stored:

![Pasted image 20240415163113](https://github.com/lm3nitro/Projects/assets/55665256/40c755e4-5815-4d9f-b85e-104190985ac2)

Finally, verifed that the Index had been created:

![Pasted image 20240415163154](https://github.com/lm3nitro/Projects/assets/55665256/0e05f7ec-ebb6-4677-911a-2b5b69718d5f)

## Splunk Fowarder and Suricata TA APP installation on server running Suricata

Suricata App:

I downloaded both the Suricata and the Splunk Forwarder from the Splunk website:

![Pasted image 20240415162408](https://github.com/lm3nitro/Projects/assets/55665256/9d372ee0-23b9-4831-9be8-5239bf487227)

Untar the file: 

![Pasted image 20240415161641](https://github.com/lm3nitro/Projects/assets/55665256/5a9e210a-0861-4d09-93f9-4a672ba00092)

Installing the deb file:
```
dpkg -i splunkforwarder-9.2.1-78803f08aabb-linux-2.6-amd64.deb 
```
Note: The binary is located under the /opt/ directory 

![Pasted image 20240415162653](https://github.com/lm3nitro/Projects/assets/55665256/f504275d-fe65-45fb-9a75-eb9e963e4cfd)

I then moved the TA-suricata-4 app to /opt/splunkforwarder/etc/apps/ 

![Pasted image 20240415162843](https://github.com/lm3nitro/Projects/assets/55665256/a86b67fa-58c7-4cc8-be37-810d3b9ed930)

Running Splunk forwarder: 
```
./splunk start
```
![Pasted image 20240415161839](https://github.com/lm3nitro/Projects/assets/55665256/580a8d4d-74b0-4dcc-ad82-28af2168970e)

![Pasted image 20240415162242](https://github.com/lm3nitro/Projects/assets/55665256/681629c7-fbdd-4b77-8250-21585727f07d)

Next, I needed to create an input file under the TA-suricata-4/default/

![Pasted image 20240415161232](https://github.com/lm3nitro/Projects/assets/55665256/a0f36578-aeb8-4ad3-879c-ca0327f3f2d7)

Input creation:

![Pasted image 20240415161332](https://github.com/lm3nitro/Projects/assets/55665256/18a4a425-8474-400e-bf04-feebcb926045)
```
[monitor:///var/log/suricata/eve.json]
host = lm3nitro 
sourcetype = suricata 
index = suricata
```
I also needed to create an output file to tell Suricata where to send the logs and the listening port I previously configured in Splunk (9997).

Output creation: Creating or editing the outputs.conf file under /opt/splunkforwarder/etc/system/local

![Pasted image 20240415163527](https://github.com/lm3nitro/Projects/assets/55665256/c7eda844-8deb-459c-b339-aab3f3f71fb5)

![Pasted image 20240415163852](https://github.com/lm3nitro/Projects/assets/55665256/82ddb40d-1aae-410a-bdaf-bd144a52342d)
```
[tcpout]
defaultGroup=suricata
[tcpout:suricata]
server=192.168.242.10:9997
```
After, I went back to Splunk to verify the logs:

Here I can see some random http traffic:

![Pasted image 20240415165302](https://github.com/lm3nitro/Projects/assets/55665256/7ffaf300-07f5-4359-afd8-af7a97086edf)

I can also see the amount of application traffic:

![Pasted image 20240415165056](https://github.com/lm3nitro/Projects/assets/55665256/1c1723b3-505c-460a-b2b4-bb6268d91cc0)

To give a higeher overview, this is a logical topology of what I have setup:

![Pasted image 20240416140904](https://github.com/lm3nitro/Projects/assets/55665256/abfd7bf9-0561-421b-b3bb-aa515b3daca6)

## Analyzing Suricata Splunk Logs

I wanted to take a look at some DNS traffic. To do this, I went to chess.com, and was able to see my traffic being logged:

![Pasted image 20240415164717](https://github.com/lm3nitro/Projects/assets/55665256/d1b85481-7a60-4619-9540-870294139528)

## IDS Alert

Next, I wanted to test and IDS alert. To do this, I created a very simple bi-directional rule to that detects ICMP traffic:
 
![Pasted image 20240415170636](https://github.com/lm3nitro/Projects/assets/55665256/fe04c431-7560-47e3-ac62-3a974fad42ac)

Now that I have created the rule, I went to Splunk to verify the alert after I purposely triggered it.

From server 1.1.1.1 to client:

![Pasted image 20240415171042](https://github.com/lm3nitro/Projects/assets/55665256/68e5ec20-83ee-41b1-9104-94c84f7c3e42)

From client 192.168.242.130 to server:

![Pasted image 20240415171445](https://github.com/lm3nitro/Projects/assets/55665256/7bb3b5dd-ceba-48e1-b725-8fbed4bc29ca)

Filtering for Alerts in Splunk:

![Pasted image 20240415171225](https://github.com/lm3nitro/Projects/assets/55665256/92ff1539-2289-468e-bc4e-cce9eee21123)

Having Suricata in my home network has added another layer of security to my network. I enjoy the features that that it has and the information contained in the fields as I filter through them in Spplunk. I also like that it allows for customization to suit your network's specific security needs, ultimately helping you proactively protect your devices and data. 
