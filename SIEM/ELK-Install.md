# Installing Elasticsearch

## Installing Dependencies and Nginx
>### Install Dependencies
```
sudo apt install default-jre
sudo apt install default-jdk
sudo apt update
sudo apt upgrade
```
>### Verify Java version
```
java -version
javac -version
```
>### Install nginx
```
sudo apt install nginx
systemctl status nginx
```
>### You can verify that it is up and running by putting the following in your browser

http://your_server_ip

>### Install Elasticsearch
```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch
```
>### During the initial installation, you will encounter the security configuration settings, which will include the default password for the elastic user. It is important to make a note of this password as it will be needed later on.

If the installation proceeds without issues, Elasticsearch will be successfully installed on the host machine.
To ensure that the Elasticsearch service persists across server restarts, the following commands will be employed.
```
systemctl enable elasticsearch
systemctl start elasticsearch
```
>### You can verify that status of Elastic search with the following command
```
systemctl status elasticsearch.service
```
## Elasticsearch Configuration

>### We will proceed to adjust the configuration to make it accessible to other components.
```
sudo nano /etc/elasticsearch/elasticsearch.yml
```
>### Change the 'network.host:' from 'localhost' to '127.0.0.1':
``
network.host: 127.0.0.1
``
>### Restart Elasticsearch
```
sudo systemctl restart elasticsearch
```
>### You can test with the following command
```
curl -X GET "localhost:9200"
```
# Install Logstash
```
sudo apt install logstash
```
>### Add persistence
```
systemctl daemon-reload
systemctl enable logstash
systemctl start logstash
```

After successfully installing Logstash, let's verify its status to ensure that it has been installed and is running correctly.
```
systemctl status logstash
```
## Logstash Configuration

By default, essential configuration files in a standard Logstash installation are located in the /etc/logstash directory.

```
nano /etc/logstash/logstash.yml
```
>### We need to make two updates. Uncomment the following variables and set the value of config.reload.automatic to true, as illustrated below:
```
config.reload.automatic: true
config.reload.interval: 3s
```
These adjustments will enable Logstash to check the configuration files every 3 seconds for any changes in the ingested log sources. Next, we wil move to installing Kibana.

# Install Kibana
```
sudo apt install kibana
```
>### To add persistence
```
sudo systemctl enable kibana
sudo systemctl start kibana
```
The usual location for the configuration files of Kibana, an open-source tool for data visualization and exploration, is within the /etc/kibana directory.
```
sudo nano /etc/kibana/kibana.yml
```
Uncomment the following two variables and make the changes to server.host as shown below:
```
server.port: 5601
server.host: "0.0.0.0"
```
Once the changes are made, and the config file is saved, restart Kibana.
```
systemctl restart kibana
```
Next, open your web browser and navigate to YOUR_IP:5601. This will bring up the Kibana interface. Next, we will generate an enrollment token for the Kibana instance using the following command:
```
/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```
This command will display a randomly generated token. Enter this token in the provided space and press Enter. You will then be prompted to enter the elastic credentials, which are the same credentials that were created during the Elasticsearch installation process.
