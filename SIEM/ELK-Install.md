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
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
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
curl -X GET "localhost:9200"
