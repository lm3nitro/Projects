# Graylog

![Pasted image 20240529170413](https://github.com/lm3nitro/Projects/assets/55665256/94243234-7b1d-4815-b502-7f179611b63b)

Is an open-source log management and analysis tool designed to help organizations collect, index, and analyze log data from various sources. It provides a centralized platform for managing log data, which can be critical for monitoring, troubleshooting, and security analysis. The software uses a three-tier architecture and scalable storage based on Elasticsearch and MongoDB. Some of the features include:

+ Log Collection
+ Indexing and Searching
+ Dashboards
+ Alerts and Notifications
+ User Management and Security
+ Plugins and Extensibility

### Scope:

In this project, I will be deploying Graylog alongside Elasticsearch, MongoDB, and Nginx to create a robust log management and analysis platform focused on analyzing NetFlow traffic. This integration will enable the collection, storage, and real-time processing of NetFlow data from various network devices, providing detailed insights into network performance and traffic patterns. Elasticsearch serves as the backend search engine for efficient indexing and querying, while MongoDB offers a flexible storage solution for configuration and metadata. 

### Tools and Technology
Wireshark, Win10, Linux, Java, Nginx, Elasticsearch, MongoDB, and Graylog

Network Diagram for this project:

![Pasted image 20240529171625](https://github.com/lm3nitro/Projects/assets/55665256/24bb3003-962e-4f3c-b09e-f610b34207ac)

## Getting Started:

Before installing anything, I wanted to ensure that all the system packages are up to date and on their latest available versions:

```
sudo apt-get update -y && sudo apt-get upgrade -y
```

![Pasted image 20240529120802](https://github.com/lm3nitro/Projects/assets/55665256/53b6bedb-05f8-479e-833d-3771be7e4daa)

## Installing Nginx:

![Pasted image 20240529174757](https://github.com/lm3nitro/Projects/assets/55665256/1ac24c7f-bbdb-4385-b5b9-b46acfe513c7)

Now that everything has been updated and upgraded, I moved on to installing Nginx:

```
sudo apt-get install nginx -y
```

![Pasted image 20240529120941](https://github.com/lm3nitro/Projects/assets/55665256/2a2c577e-7913-4de8-bc81-12f70e8b3a4e)

After successful installation, the Nginx service will be automatically started. To check the status of Nginx, execute the following command:

```
sudo systemctl status nginx
```

![Pasted image 20240529121035](https://github.com/lm3nitro/Projects/assets/55665256/451a612b-95ff-4700-a2fb-55d01e4073a3)

## Installing MongoDB:

MongoDB is a popular open-source NoSQL database designed for storing and managing large volumes of unstructured or semi-structured data.

![Pasted image 20240529174041](https://github.com/lm3nitro/Projects/assets/55665256/c731c168-6333-4177-8a6b-ea4651c6e1d8)


```
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
```

I then added the MongoDB repository:

```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list
```

![Pasted image 20240529121438](https://github.com/lm3nitro/Projects/assets/55665256/b97e05f6-0b46-4b18-9a99-7ca84a7699dd)

After, I updated and upgraded the packages again:

```
sudo apt update -y
sudo apt upgrade -y
sudo apt-get install gnupg libssl1.1 -y
```

![Pasted image 20240529121422](https://github.com/lm3nitro/Projects/assets/55665256/1f30bd69-c84f-4759-8955-f9bafb8c3800)

Next, I installed MongoDB:

```
sudo apt-get install mongodb-org=4.4.8 mongodb-org-server=4.4.8 mongodb-org-shell=4.4.8 mongodb-org-mongos=4.4.8 mongodb-org-tools=4.4.8 -y
```

![Pasted image 20240529121528](https://github.com/lm3nitro/Projects/assets/55665256/a48db1ed-4c0d-4d6f-a4d5-18449614512d)

Started and enabled the MongoDB service:

```
sudo systemctl start mongod && sudo systemctl enable mongod
```

To check the status of MongoDB, execute the command below:

```
sudo systemctl status mongod
```
You should receive the following output:

![Pasted image 20240529121631](https://github.com/lm3nitro/Projects/assets/55665256/0df1e08b-590b-4633-98be-a420cb4e4d07)

## Installing Java:

![Pasted image 20240529174125](https://github.com/lm3nitro/Projects/assets/55665256/dd9d532e-0bf4-43c2-95d8-90e31227911c)

> [!NOTE]  
> To install the latest Java version, I first neede to install some Java dependencies:

```
apt install apt-transport-https gnupg2 uuid-runtime pwgen curl dirmngr -y
```

![Pasted image 20240529121743](https://github.com/lm3nitro/Projects/assets/55665256/5dfbf94a-9651-4010-bd69-c5a85a827ad0)

After installing the dependencies, I then installed Java with the following command:

```
apt install openjdk-11-jre-headless -y
```

![Pasted image 20240529121842](https://github.com/lm3nitro/Projects/assets/55665256/a49af932-61e5-43e5-b714-be5925de87d4)

After successful installation, I checked the installed Java version:

```
java --version
```

![Pasted image 20240529121929](https://github.com/lm3nitro/Projects/assets/55665256/1a57a4d8-6e5d-456f-8208-1d18df0b3de2)

## Installing Elasticsearch:

![Pasted image 20240529174152](https://github.com/lm3nitro/Projects/assets/55665256/487e70c4-c391-487c-9dbd-dc36a0c07d62)

First, I needed to add the elasticsearch public key to the APT, and the elastic source to the sources.list.d. To add the GPG-KEY execute the following command:

```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
```

![Pasted image 20240529122021](https://github.com/lm3nitro/Projects/assets/55665256/121ea224-40cc-471b-80c0-0824cee7549e)

To add the elastic source in the sources.list.d execute the following command:

```
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

![Pasted image 20240529122057](https://github.com/lm3nitro/Projects/assets/55665256/c65eb0a5-b324-4ec9-8d5a-40ce717219ef)

Now, I updated the system and installed elasticsearch with the following commands:

```
sudo apt update -y
sudo apt install elasticsearch
```

![Pasted image 20240529122151](https://github.com/lm3nitro/Projects/assets/55665256/752ede93-a1a7-4545-900c-9907908c22ee)

I then started and enabled the elasticsearch service:

```
sudo systemctl start elasticsearch && sudo systemctl enable elasticsearch
```

![Pasted image 20240529122254](https://github.com/lm3nitro/Projects/assets/55665256/c94cb4ae-724f-436b-8f48-949e6908b182)

To check the status and see if the service is up and running, execute the following command:

```
sudo systemctl status elasticsearch
```

![Pasted image 20240529122324](https://github.com/lm3nitro/Projects/assets/55665256/0223c5f6-da83-4b10-98c9-4107001dc0bd)

After starting the service, I then configured the cluster name for our Graylog server:

```
cluster.name: graylog
action.auto_create_index: false
```

![Pasted image 20240529122658](https://github.com/lm3nitro/Projects/assets/55665256/5ac5f53c-eb1e-43fd-b7df-bbe8e2371592)

I Saved the file, closed it and restarted the daemon along with elasticsearch service:

```
sudo systemctl daemon-reload && sudo systemctl restart elasticsearch
```

![Pasted image 20240529122801](https://github.com/lm3nitro/Projects/assets/55665256/cf2c0d96-b62a-4459-9dce-5f9336cb18ea)

## Install Graylog Server:

![Pasted image 20240529174239](https://github.com/lm3nitro/Projects/assets/55665256/0bd2dbe4-98aa-4fe4-9f44-7072ac96c97f)

I have installed MongoDB, Java, Elasticsearch, and Nginx. I now moved on to Graylog and downloaded it:

```
wget https://packages.graylog2.org/repo/packages/graylog-4.3-repository_latest.deb
```

![Pasted image 20240529124759](https://github.com/lm3nitro/Projects/assets/55665256/2c3ccfb2-9ed4-45d9-8487-c2d0e184ac91)

Unpacked it:

```
dpkg -i graylog-4.3-repository_latest.deb
```

![Pasted image 20240529124850](https://github.com/lm3nitro/Projects/assets/55665256/9037f5f8-58b6-4bc8-abb1-49017c36961a)

Ensured that everything was updated:

```
sudo apt update -y
```

![Pasted image 20240529124926](https://github.com/lm3nitro/Projects/assets/55665256/28028dc6-7cbc-4091-af77-ab2e316b3653)

Now installed it:

```
sudo apt install graylog-server -y
```

![Pasted image 20240529125006](https://github.com/lm3nitro/Projects/assets/55665256/c5821de0-629c-4a87-b8d4-b5d448534581)

Next, I started and enabled the graylog-server service:

```
systemctl enable graylog-server.service && systemctl start graylog-server.service
```

![Pasted image 20240529125103](https://github.com/lm3nitro/Projects/assets/55665256/7c485832-a6e8-41e7-aef2-969473b541f7)

Checked the status of the Graylog server:

```
systemctl status graylog-server
```

![Pasted image 20240529125321](https://github.com/lm3nitro/Projects/assets/55665256/3deb02f8-b873-4404-a7a6-10526a130237)

Once installed, I then configured a Graylog User:

In this step I will secure the user password using the password generator command pwgen.

```
pwgen -N 1 -s 96
```

![Pasted image 20240529125415](https://github.com/lm3nitro/Projects/assets/55665256/4190c54b-002e-4144-b238-52caf23e0e4b)

Password:
cFQUb7duLJoW5YlZx0CH109LC9pukmJpjnbogWpD5FGdPvYci1Qfp73ba0z2XS0hKQ35JwfiSBij1HtwXYNCnTKGAzhKOYSD

I also needed to create an admin password:

```
echo -n lm3nitro | shasum -a 256
```

![Pasted image 20240529135341](https://github.com/lm3nitro/Projects/assets/55665256/fd893674-681f-4944-b527-b9421d5843a9)

Password:
54a3bb2a82bb6a098aa53cb6c343445b39ba4078bbe5ad1a8578a3a877882590 -

Next, I opened the Graylog configuration file located in `/etc/graylog/server/server.conf` and found the part refrencing password_secret and root_password_sha2 fields. I pasted the previously generated passwords.

Note:  Remove the " - " and the spaces from the root password:

![Pasted image 20240529135721](https://github.com/lm3nitro/Projects/assets/55665256/1c26cccd-7944-4e2f-a1e7-6dc048458170)

Once complete, I saved/closed it and restarted the graylog server.

```
systemctl daemon-reload
systemctl restart graylog-server
systemctl status graylog-server
```

![Pasted image 20240529130650](https://github.com/lm3nitro/Projects/assets/55665256/ab0dc341-15bb-463d-8e03-88ecf7fd9a12)

## Create Nginx Virtual Host:

An Nginx virtual host (also known as a server block) is a configuration that allows Nginx to host multiple websites or applications on a single server.

To do this, I first went into the Graylog configuration file:
```
touch /etc/nginx/sites-available/graylog.conf
```

![Pasted image 20240529130811](https://github.com/lm3nitro/Projects/assets/55665256/55da128d-b876-41a3-ad30-f7aa190b6227)

I opened the file and pasted the following lines of code:

```
server {
 listen 80;
 server_name lm3nitro.graylog.local;

 location / {
   proxy_set_header Host $http_host;
   proxy_set_header X-Forwarded-Host $host;
   proxy_set_header X-Forwarded-Server $host;
   proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   proxy_set_header X-Graylog-Server-URL http://$server_name/;
   proxy_pass http://127.0.0.1:9000;
   }
}
```

> [!IMPORTANT]  
> proxy_pass http://127.0.0.1:9000 is running on port 9000 

![Pasted image 20240529140713](https://github.com/lm3nitro/Projects/assets/55665256/72b7c985-31e3-4c2e-b57c-57d509f252b8)

I verified the IP of the server:

```
ip add
```

![Pasted image 20240529130904](https://github.com/lm3nitro/Projects/assets/55665256/ad35c815-eb32-495f-a5b0-18b5a401cd9e)

![Pasted image 20240529131111](https://github.com/lm3nitro/Projects/assets/55665256/6b3bf6b6-7286-498c-bc00-1e407d5fe927)


Next, I created a static domain entry for `http://lm3nitro.graylog.local/ under C:\Windows\System32\drivers\etc\hosts`  in my Win10 host:

![Pasted image 20240529140046](https://github.com/lm3nitro/Projects/assets/55665256/1f77d99f-39d4-4beb-b171-8ac10ebc11da)

I then enabled the Nginx configuration with a symbolic link:

```
ln -s /etc/nginx/sites-available/graylog.conf /etc/nginx/sites-enabled/
```

![Pasted image 20240529131159](https://github.com/lm3nitro/Projects/assets/55665256/2f978351-2e7a-47ab-a86c-24c408ce1206)

Checked the Nginx syntax:

![Pasted image 20240529131905](https://github.com/lm3nitro/Projects/assets/55665256/e75c5fbe-d869-4d5c-ba39-c03f9fce57dc)

Nginx status:

```
systemctl restart nginx
systemctl status nginx
```

![Pasted image 20240529131944](https://github.com/lm3nitro/Projects/assets/55665256/cd43b76f-716f-4a85-a1a3-a03b6122e095)

After the installation and configuration, I was able to access the Graylog server at `http://lm3nitro.graylog.local` using the credentials created above.

Graylog WebUI:

![Pasted image 20240529140409](https://github.com/lm3nitro/Projects/assets/55665256/60c3ca54-2866-48c1-b4f6-39dba11034ca)

Checking the Nginx logs, I can see the the traffic that was generated:

![Pasted image 20240529141054](https://github.com/lm3nitro/Projects/assets/55665256/1f3f2c25-8bd2-4955-9e15-9fdc39798c70)

Also, looking at Wireshark, I can see the login:

![Pasted image 20240529141644](https://github.com/lm3nitro/Projects/assets/55665256/821c7bea-8cf0-48ea-95da-8bf8c33f2cab)

As seen below, there is no data yet:

![Pasted image 20240529140605](https://github.com/lm3nitro/Projects/assets/55665256/f691c172-20fe-4d64-88be-0e6bc75d060c)

## Neflow configuration:

Download the plugin under `/usr/share/graylog-server/plugin`

```
wget https://github.com/Graylog2/graylog-plugin-netflow/releases/download/0.1.1/graylog-plugin-netflow-0.1.1.jar
```
![Pasted image 20240529145841](https://github.com/lm3nitro/Projects/assets/55665256/aa0c3f37-2ebb-402f-8901-0772241098fb)

Reload the Graylolog server:

```
systemctl daemon-reload
systemctl restart graylog-server
```

## Creating a Netflow input:

In Graylog I went to System > Inputs and selected NetFlow UDP from the drop down and filled out the fields;

![Pasted image 20240529150224](https://github.com/lm3nitro/Projects/assets/55665256/7e226d26-316f-43cb-8c30-779728668309)

![Pasted image 20240529150346](https://github.com/lm3nitro/Projects/assets/55665256/dddb678b-7c59-408b-ac36-a7a21177a21c)

In the image below, on the upper left-hand side I have traffic information about the NetFlow Data sent to the Graylog sever on 172.16.10.10 from Cisco Router "lm3nitro-r1" on 172.16.10.1. 

On the upper right-hand side there is network information about the Windows 10 PC. The bottom left-hand shows information on the Graylog server and the lower right-hand side shows the Cisco router NetFlow cache at the moment of the screenshot:

![Pasted image 20240529173507](https://github.com/lm3nitro/Projects/assets/55665256/ea9f6fd6-a2f6-4266-bd8f-3f8f6af43565)

I also verified in Graylog the NetFlow messages that were coming from my Cisco router:

![Pasted image 20240529145622](https://github.com/lm3nitro/Projects/assets/55665256/7ff16b04-25d7-49f1-8da5-35b7f5079ac7)

## NetFlow Analysis

1. Traffic information from my Windows 10 PC:

![Pasted image 20240529145719](https://github.com/lm3nitro/Projects/assets/55665256/012e29bd-1dce-4d72-a4c7-17320ae4c1ca)

2. Querying a destination IP and destination port:

![Pasted image 20240529152637](https://github.com/lm3nitro/Projects/assets/55665256/da8f4277-a76d-47e4-9a0f-f96818faad43)

3. Querying source IP and exporting csv file:

![Pasted image 20240529153236](https://github.com/lm3nitro/Projects/assets/55665256/e21c100a-e37e-452e-b912-96b70af8649f)

4. Using Sublime to search for IP addresses:

![Pasted image 20240529153836](https://github.com/lm3nitro/Projects/assets/55665256/64f7a162-c227-46c5-900b-fe4ac469b034)

5. Selecting all time traffic:

![Pasted image 20240529174916](https://github.com/lm3nitro/Projects/assets/55665256/d10b7236-12cc-488c-a111-0840d0fe0a44)

6. Statistical view of the NetFlow data:

![Pasted image 20240529212322](https://github.com/lm3nitro/Projects/assets/55665256/b22fae66-b8b7-4b3b-a5c0-425149f2702d)

### Summary:

By Installing and Configuring Graylog along with Elasticsearch, MongoDB, and other components to analyze NetFlow traffic, I gained a deep understanding of centralized log management and network traffic monitoring. Graylog acted as the main log collector, processing and analyzing NetFlow data in real time, while Elasticsearch indexed and retrieved this data efficiently for querying and filtering. MongoDB stored the metadata and configurations that supported Graylog’s search functionality. 

This hands-on experience taught me the importance of network traffic analysis for security monitoring, anomaly detection, and performance optimization. I also grasped how each component played a critical role in a distributed logging system, especially in terms of data retention, query performance, and visualization of network flows. The project also helped me understand how to seamlessly integrate these tools for effective traffic pattern monitoring. By having a unified system, I was able to efficiently analyze traffic flows, allowing me the ability to detect potential threats, and gain real-time insights that are essential for maintaining a stable and secure network environment.
