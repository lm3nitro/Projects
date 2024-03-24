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
>### You can test with the following command
```
curl -X GET "localhost:9200"
```
sudo apt install kibana
sudo systemctl enable kibana
sudo systemctl start kibana

echo "elastic:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users
#U:elastic P:$N34i67f3!

sudo nano /etc/nginx/sites-available/172.16.10.50

server {
    listen 80;

    server_name 172.16.10.50;

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


**************

sudo ln -s /etc/nginx/sites-available/172.16.10.50 /etc/nginx/sites-enabled/172.16.10.50
sudo nginx -t


sudo apt install logstash
sudo nano /etc/logstash/conf.d/02-beats-input.conf

input {
  beats {
    port => 5044
  }
}

*************

sudo nano /etc/logstash/conf.d/30-elasticsearch-output.conf
output {
  if [@metadata][pipeline] {
	elasticsearch {
  	hosts => ["localhost:9200"]
  	manage_template => false
  	index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
  	pipeline => "%{[@metadata][pipeline]}"
	}
  } else {
	elasticsearch {
  	hosts => ["localhost:9200"]
  	manage_template => false
  	index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
	}
  }
}


*************

sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
sudo systemctl start logstash
sudo systemctl enable logstash

