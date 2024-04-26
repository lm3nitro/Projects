# Database Installation

Real Intelligence Threat Analytics (RITA) is an open-source framework designed for network traffic analysis and threat hunting. I will be installing RITA in my home network and will be using it to detect and respond to threats. I am hoping to get insights into my network traffic and see more of RITA's capabilities.

There are different components needed to install RITA properly. Here is the process that I used to get it installed. 

### Getting Started

1. Import the public key 
First, I will need to install the public key used by the management system:
```
sudo apt-get install gnupg
```
![Pasted image 20240417111515](https://github.com/lm3nitro/Projects/assets/55665256/bd445abe-36b1-4685-b3e2-4f77eb809380)

2. Import the MongoDB public GPG Key:

```
curl -fsSL https://pgp.mongodb.com/server-4.2.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-4.2.gpg \
   --dearmor
```
![Pasted image 20240417112228](https://github.com/lm3nitro/Projects/assets/55665256/5393d35e-f6fb-4867-9ed2-650d10061a81)

3. Create a /etc/apt/sources.list.d/mongodb-org-4.2.list file for MongoDB :
```
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-4.2.gpg ] http://repo.mongodb.org/apt/debian buster/mongodb-org/4.2 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```
![Pasted image 20240417112240](https://github.com/lm3nitro/Projects/assets/55665256/34687981-c111-4eb8-a3b8-91fdfe409393)

4. Reload local package database:
```
sudo apt-get update
```

![Pasted image 20240417112253](https://github.com/lm3nitro/Projects/assets/55665256/30ba1278-db34-4354-99b3-a72c2208e64b)

### Install the MongoDB packages:

Next, I will need to install MongoDB. MongoDB is an open-source DB. RITA uses MongoDB as its database to store and manage the network flow data, DNS logs, and other metadata collected during network traffic analysis. 
```
sudo apt-get install -y mongodb-org
```
![Pasted image 20240417112306](https://github.com/lm3nitro/Projects/assets/55665256/95ed4fdd-8d53-4c15-ab02-a13d6398f983)

1. Start MongoDB:
```
sudo systemctl start mongod
```
![Pasted image 20240417112719](https://github.com/lm3nitro/Projects/assets/55665256/91983b05-12dc-4d0c-b7cc-f982bec6a9c9)

> #### Note: If you receive an error similar to the following when starting mongod:
> "Failed to start mongod.service: Unit mongod.service not found".
> #### Run the following command first:
