# Graylog


![Pasted image 20240529170413](https://github.com/lm3nitro/Projects/assets/55665256/94243234-7b1d-4815-b502-7f179611b63b)


The Graylog software centrally captures, stores, and enables real-time search and log analysis against terabytes of machine data from any component in the IT infrastructure and applications. The software uses a three-tier architecture and scalable storage based on Elasticsearch and MongoDB.


Network Diagram:



![Pasted image 20240529171625](https://github.com/lm3nitro/Projects/assets/55665256/24bb3003-962e-4f3c-b09e-f610b34207ac)

# Update the System:

updating the system packages to their latest versions available.

![Pasted image 20240529120802](https://github.com/lm3nitro/Projects/assets/55665256/53b6bedb-05f8-479e-833d-3771be7e4daa)


# Install Nginx:


![Pasted image 20240529174757](https://github.com/lm3nitro/Projects/assets/55665256/1ac24c7f-bbdb-4385-b5b9-b46acfe513c7)



sudo apt-get install nginx -y


20240529120941------------


After successful installation, the Nginx service will be automatically started. To check the status of Nginx, execute the following command:

sudo systemctl status nginx

![[Attachments/Pasted image 20240529121035.png]]

# Install MongoDB Database Server:




![[Attachments/Pasted image 20240529174041.png]]



Adding the GPG keys:

wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -


Then, we need to add the MongoDB repository:


echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list


![[Attachments/Pasted image 20240529121438.png]]

Then, we need to add the MongoDB repository:



sudo apt update -y
sudo apt upgrade -y
sudo apt-get install gnupg libssl1.1 -y


![[Attachments/Pasted image 20240529121422.png]]




sudo apt-get install mongodb-org=4.4.8 mongodb-org-server=4.4.8 mongodb-org-shell=4.4.8 mongodb-org-mongos=4.4.8 mongodb-org-tools=4.4.8 -y

![[Attachments/Pasted image 20240529121528.png]]

After this start and enable the MongoDB service:

sudo systemctl start mongod && sudo systemctl enable mongod


To check the status of MongoDB execute the command below:


sudo systemctl status mongod


You should receive the following output:


![[Attachments/Pasted image 20240529121631.png]]

#  Install Java:


![[Attachments/Pasted image 20240529174125.png]]


Note:  To install the latest Java version, we need to install first some Java dependencies:


apt install apt-transport-https gnupg2 uuid-runtime pwgen curl dirmngr -y



![[Attachments/Pasted image 20240529121743.png]]

Once these dependencies are installed, we can install Java with the following command:


apt install openjdk-11-jre-headless -y


![[Attachments/Pasted image 20240529121842.png]]


After successful installation, check the installed Java version:

java --version

![[Attachments/Pasted image 20240529121929.png]]



# Install Elasticsearch:


![[Attachments/Pasted image 20240529174152.png]]


First we are going to add the elasticsearch public key to the APT, and the elastic source to the sources.list.d.


To add the GPG-KEY execute the following command:

curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg


![[Attachments/Pasted image 20240529122021.png]]

To add the elastic source in the sources.list.d execute the following command:

echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list



![[Attachments/Pasted image 20240529122057.png]]

Now, update the system and install the elastic search with the following commands:




sudo apt update -y

sudo apt install elasticsearch



![[Attachments/Pasted image 20240529122151.png]]

Start and enable the elasticsearch service:



sudo systemctl start elasticsearch && sudo systemctl enable elasticsearch

![[Attachments/Pasted image 20240529122254.png]]

To check the status of the service if is up and running execute the following command:

sudo systemctl status elasticsearch


![[Attachments/Pasted image 20240529122324.png]]


After starting the service we need to configure the cluster name for our Graylog server:

cluster.name: graylog
action.auto_create_index: false

![[Attachments/Pasted image 20240529122658.png]]

Save the file, close it and restart the daemon along with elasticsearch service:


![[Attachments/Pasted image 20240529122801.png]]


# Install Graylog Server:


![[Attachments/Pasted image 20240529174239.png]]



wget https://packages.graylog2.org/repo/packages/graylog-4.3-repository_latest.deb


![[Attachments/Pasted image 20240529124759.png]]

After that, we need to install it:

dpkg -i graylog-4.3-repository_latest.deb


![[Attachments/Pasted image 20240529124850.png]]


sudo apt update -y



![[Attachments/Pasted image 20240529124926.png]]


sudo apt install graylog-server -y


![[Attachments/Pasted image 20240529125006.png]]



Start and Enable the graylog-server service:


systemctl enable graylog-server.service && systemctl start graylog-server.service


![[Attachments/Pasted image 20240529125103.png]]



To check the status of the Graylog server execute the following command:




systemctl status graylog-server



![[Attachments/Pasted image 20240529125321.png]]




Configure Graylog User:


In this step we will secure the user passwords using the password generator command pwgen.



pwgen -N 1 -s 96

![[Attachments/Pasted image 20240529125415.png]]

Password:
cFQUb7duLJoW5YlZx0CH109LC9pukmJpjnbogWpD5FGdPvYci1Qfp73ba0z2XS0hKQ35JwfiSBij1HtwXYNCnTKGAzhKOYSD

Then we will create an admin password:

![[Attachments/Pasted image 20240529135341.png]]


Password:
54a3bb2a82bb6a098aa53cb6c343445b39ba4078bbe5ad1a8578a3a877882590  -



Open the /etc/graylog/server/server.conf file and find the part password_secret and root_password_sha2 fields. Paste the previously generated passwords.


Note:  Remove the " - " and the spaces from the root password:


![[Attachments/Pasted image 20240529135721.png]]




Save the file, close it and restart the graylog server.

systemctl daemon-reload

systemctl restart graylog-server

systemctl status graylog-server

![[Attachments/Pasted image 20240529130650.png]]


Create Nginx Virtual Host:


touch /etc/nginx/sites-available/graylog.conf


![[Attachments/Pasted image 20240529130811.png]]


Open the file and paste the following lines of code:


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




Note:

proxy_pass http://127.0.0.1:9000 is running on port 9000 

![[Attachments/Pasted image 20240529140713.png]]

Verify the IP of the server:

![[Attachments/Pasted image 20240529130904.png]]



![[Attachments/Pasted image 20240529133111.png]]

Creating a static domain entry for http://lm3nitro.graylog.local/ under C:\Windows\System32\drivers\etc\hosts  in  Windows 10 if doesn't have  DNS server:

![[Attachments/Pasted image 20240529140046.png]]


Enable the Nginx configuration with a symbolic link:



ln -s /etc/nginx/sites-available/graylog.conf /etc/nginx/sites-enabled/



![[Attachments/Pasted image 20240529131159.png]]

Check the Nginx syntax:

![[Attachments/Pasted image 20240529131905.png]]



Nginx status:


![[Attachments/Pasted image 20240529131944.png]]



 Access your Graylog server at http://lm3nitro.graylog.local using the credentials you created above.



Checking the Nginx logs:

![[Attachments/Pasted image 20240529141054.png]]

Traffic logs:


![[Attachments/Pasted image 20240529141644.png]]




Graylog WebUI:



![[Attachments/Pasted image 20240529140409.png]]

There is not data yet:

![[Attachments/Pasted image 20240529140605.png]]




# Neflow configuration:




Download the plugin under /usr/share/graylog-server/plugin


wget https://github.com/Graylog2/graylog-plugin-netflow/releases/download/0.1.1/graylog-plugin-netflow-0.1.1.jar

![[Attachments/Pasted image 20240529145841.png]]


Reload the Graylolog server:


systemctl daemon-reload

systemctl restart graylog-server



# Creating a Netflow input:


In Graylog go to System > Inputs and select NetFlow UDP from the drop down.



![[Attachments/Pasted image 20240529150224.png]]

![[Attachments/Pasted image 20240529150346.png]]







On the left side we have traffic Information about the NetFlow Data send to the Graylog sever on 172.16.10.10 from Cisco Router "lm3nitro-r1" 172.16.10.1.  On the right side network  information about the Windows 10 PC  and the bottom The Graylog server and the Cisco router Neff low cache at the moment of the screenshot 

![[Attachments/Pasted image 20240529173507.png]]

Verify the NetFlow messages are coming from Cisco router:



![[Attachments/Pasted image 20240529145622.png]]


Detail information about NetFlow record:


We can see traffic information from our Windows 10 PC:



![[Attachments/Pasted image 20240529145719.png]]


Query a destination IP and destination port:


![[Attachments/Pasted image 20240529152637.png]]



Query source IP and exporting csv file:

![[Attachments/Pasted image 20240529153236.png]]


Using Sublime to search for IP addresses:

![[Attachments/Pasted image 20240529153836.png]]



Selecting all time traffic:

![[Attachments/Pasted image 20240529174916.png]]



Statistical view of the NetFlow data:

![[Attachments/Pasted image 20240529212322.png]]
