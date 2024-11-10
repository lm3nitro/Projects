# ELK

<img width="419" alt="Screenshot 2024-04-27 at 2 05 30 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/7c35c537-0af4-45e2-be18-e6ea7998d194">

ELK stands for Elasticsearch, Logstash, and Kibana. ELK is a collection of open-source projects that work together as a management and analysis platform and can be used as a SIEM solution. Elasticsearch quickly searches through logs and data, Logstash collects and organizes this data, and Kibana then presents it in easy-to-understand visuals, like graphs and charts. Some of the benefits are:

+ Centralized Logging
+ Real-Time Monitoring
+ Search and Analysis
+ Visualization
+ Incident Response

### Scope:
This project will encompass the installation and configuration on ELK on a Linux VM. This will be used to gain hands on experience with ELK, understand the different components and will be used later on to create and test specific use cases for security monitoring and threat detection. 

### Tools and Technology:
Linux, Nginx, Elasticsearch, Logstash, and Kibana

## Installing Dependencies

To get started, I needed to install the dependencies.

```
sudo apt install default-jre
sudo apt install default-jdk
sudo apt update
sudo apt upgrade
```
Verify Java version
```
java -version
javac -version
```

## Install Nginx

Next, I will need to install Nginx. Nginx is a high-performance web server with different functions (reverse proxy, load balance, etc.). Nginx can be installed using the following commands:

```
sudo apt install nginx
systemctl status nginx
```

You can verify that it is up and running by putting the following in your browser

http://your_server_ip

## Install Elasticsearch

Now that I have Nginx and the dependencies installed, I proceeded to install Elasticsearch. 

```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch

```
>### Note: During the initial installation, you will encounter the security configuration settings, which will include the default password for the elastic user. It is important to make a note of this password as it will be needed later on.

If the installation proceeds without issues, Elasticsearch will be successfully installed on the host machine.

To ensure that the Elasticsearch service persists upon server restarts, the following commands can be used:

```
systemctl enable elasticsearch
systemctl start elasticsearch
```

I verified the status of Elastic search with the following command

```
systemctl status elasticsearch.service
```

## Elasticsearch Configuration

I then adjusted the configuration to make it accessible to other components.

```
sudo nano /etc/elasticsearch/elasticsearch.yml
```
Change the 'network.host:' from 'localhost' to '127.0.0.1':

``
network.host: 127.0.0.1
``
Restart Elasticsearch

```
sudo systemctl restart elasticsearch
```
You can test with the following command

```
curl -X GET "localhost:9200"
```

# Install Logstash

Once Elasticsearch has been installed, it is time to install logstash:

```
sudo apt install logstash
```
I then added persistence using the commands below:

```
systemctl daemon-reload
systemctl enable logstash
systemctl start logstash
```

After successfully installing Logstash, I verified its status to ensure that it has been installed and is running correctly.

```
systemctl status logstash
```

## Logstash Configuration

By default, essential configuration files in a standard Logstash installation are located in the /etc/logstash directory.

```
nano /etc/logstash/logstash.yml
```

I made two updates. I Uncommented the following variables and set the value of config.reload.automatic to true, as illustrated below:

```
config.reload.automatic: true
config.reload.interval: 3s
```
These adjustments will enable Logstash to check the configuration files every 3 seconds for any changes in the ingested log sources. Next, we wil move to installing Kibana.

# Install Kibana

Next, I needed to isntall Kibana. 

```
sudo apt install kibana
```

I added persistence as I did with Elasticserach and Logstash above:

```
sudo systemctl enable kibana
sudo systemctl start kibana
```

The usual location for the configuration files of Kibana, an open-source tool for data visualization and exploration, is within the /etc/kibana directory.

```
sudo nano /etc/kibana/kibana.yml
```

I uncomment the following two variables and make the changes to server.host as shown below:

```
server.port: 5601
server.host: "0.0.0.0"
```

Once the changes are made, and the config file is saved, I restarted Kibana"

```
systemctl restart kibana
```

Next, open your web browser and navigate to YOUR_IP:5601. This will bring up the Kibana interface. Next, we will generate an enrollment token for the Kibana instance using the following command:

```
/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```
![Screenshot 2024-09-21 at 4 16 34 PM](https://github.com/user-attachments/assets/3c0190c9-7e58-496a-bf10-2ea59af53d61)

This command will display a randomly generated token. Enter this token in the provided space and press Enter. You will then be prompted to enter the elastic credentials, which are the same credentials that were created during the Elasticsearch installation process.

### Summary:

In this project I was able to install the ELK stack along with Nginx on my Linux VM. I will be using ELK for future projects to test and perform various use cases. This will in turn allow me to assess how well the ELK stack handles the incoming information and how it parses through the logs. Having ELK installed in my network will also allow me to integrate it with other tools and data sources for enhanced security and functionality. 

