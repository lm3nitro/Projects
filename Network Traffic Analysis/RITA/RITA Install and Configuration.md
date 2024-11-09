# RITA

![Screenshot 2024-04-27 at 10 46 35 PM](https://github.com/lm3nitro/Projects/assets/55665256/db334ca5-ffa9-44bc-a9b0-28cc1ac98b1d)

Real Intelligence Threat Analytics (RITA) is an open-source framework designed for network traffic analysis and threat hunting. I will be installing RITA in my home network and will be using it to detect and respond to threats. I am hoping to get insights into my network traffic and see more of RITA's capabilities.

There are different components needed to install RITA properly. Here is the process that I used to get it installed. 

### Scope:
I will be combining RITA, Zeek, and Suricata on a single server to create a robust threat hunting platform where Suricata acts as an IDS/IPS, Zeek conducts in-depth packet analysis and generates comprehensive network logs, and RITA correlates and analyzes network flow data to detect patterns indicative of malicious behavior. I will then use this combination to perform threat hunting in the next section. 

### Tools and Technology:
Rita, Linux OS, MongoDB, Suricata, and Zeek

## Getting Started

1. Import the public key 
First, I will need to install the public key used by the management system:
```
sudo apt-get install gnupg
```
![Pasted image 20240417111515](https://github.com/lm3nitro/Projects/assets/55665256/bd445abe-36b1-4685-b3e2-4f77eb809380)

2. Import the MongoDB public GPG Key:

![Screenshot 2024-04-27 at 10 48 38 PM](https://github.com/lm3nitro/Projects/assets/55665256/79b87724-8cb9-40cd-96e9-e32bf9255240)


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

2. Verify that MongoDB has started successfully:
```
sudo systemctl status mongod
```
![Pasted image 20240417112840](https://github.com/lm3nitro/Projects/assets/55665256/656219c2-dcb9-46e2-9324-d3aea55067a8)

> #### The following are optional commands to enable, start, stop, and restart MongoDB

1. Make it so that MongoDB will start following a system reboot by issuing the following command:
```
sudo systemctl enable mongod
```
![Pasted image 20240417112906](https://github.com/lm3nitro/Projects/assets/55665256/3cef96ae-13ee-4e5e-965e-c88de89757cd)

2. Stop MongoDB
Stop the mongod process by issuing the following command:
```
sudo systemctl stop mongod
```
3. Restart MongoDB
Restart the mongod process by issuing the following command:
```
sudo systemctl restart mongod
```

### Rita Building from Source
 
1. Installing Golang
In order to compile RITA manually I needed to install Golang (v1.13 or greater). First, I installed the package:

```
sudo apt install -y golang
```
Then add the following to your .bashrc
```
export GOROOT=/usr/lib/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```
Reload your .bashrc
```
source .bashrc
```
A look at the Logs:

![Pasted image 20240417113744](https://github.com/lm3nitro/Projects/assets/55665256/c4654717-f1a3-43d1-9092-e0bdb4d8d19a)


### Building RITA

1. At this point you can build RITA from source code.
```
git clone https://github.com/activecm/rita.git
cd rita
make 
```
>#### You will need to have make installed. You can use your system's package manager to install it if needed.

![Pasted image 20240417113130](https://github.com/lm3nitro/Projects/assets/55665256/f1f27727-cde3-46bb-ba57-366c67128b8c)
![Pasted image 20240417113206](https://github.com/lm3nitro/Projects/assets/55665256/f8fbbe0c-1ed8-4510-bdd1-775dc1367d35)

2. Configuring the system
RITA requires a few directories to be created for it to function correctly.
```
sudo mkdir /etc/rita && sudo chmod 755 /etc/rita
sudo mkdir -p /var/lib/rita/logs && sudo chmod -R 755 /var/lib/rita
sudo chmod 777 /var/lib/rita/logs
```

3. Copy the config file from your local RITA source code.
```
sudo cp etc/rita.yaml /etc/rita/config.yaml && sudo chmod 666 /etc/rita/config.yaml
```
![Pasted image 20240417113315](https://github.com/lm3nitro/Projects/assets/55665256/73231609-c7b0-46d0-a40b-78ec4e148215)

4. At this point, you can modify the config file as needed and test using the rita test-config command.

### Verify Rita Version:

![Pasted image 20240417150321](https://github.com/lm3nitro/Projects/assets/55665256/4847afcf-0300-44b0-9b22-dee8584bad7c)

>#### Note: RITA itself only puts files in a few places: `/usr/local/bin/rita`, `/etc/rita/`, and `/var/lib/rita/`.  If you want remove Rita, you can just directly delete all those.

## Upgrading Between Minor or Patch Versions:

If you are upgrading within the same major version (e.g. v2.0.0 to v2.0.1, or v3.0.0 to v3.1.0) all you need to do is download the newest RITA binary and replace the one on your system.

Example: 
Replace the binary located `/usr/local/bin/rita` with the new binary.

![Pasted image 20240419110545](https://github.com/lm3nitro/Projects/assets/55665256/1aefe3e4-495b-4209-83a4-1f9af926ec46)

# Zeek  

![Screenshot 2024-04-27 at 10 49 02 PM](https://github.com/lm3nitro/Projects/assets/55665256/4fd56441-9ada-4689-a946-7e3d9117d70e)

Next, I also wanted to install Zeek on the same server as RITA, and these are the steps needed to do that:

### Building from Source:

1. Install required dependencies:
```
sudo apt-get install cmake make gcc g++ flex libfl-dev bison libpcap-dev libssl-dev python3 python3-dev swig zlib1g-dev
```
![Pasted image 20240417114644](https://github.com/lm3nitro/Projects/assets/55665256/c56054f7-e416-456d-aa02-46521b037a4a)

2. Optional Dependencies
```
sudo apt-get install python3-git python3-semantic-version
```
![Pasted image 20240417114721](https://github.com/lm3nitro/Projects/assets/55665256/79e4704a-2af2-46ae-87ec-07b60982af60)

3. Retrieving the Sources
```
git clone --recurse-submodules https://github.com/zeek/zeek
```
![Pasted image 20240417114851](https://github.com/lm3nitro/Projects/assets/55665256/15dc6bda-d48f-481c-b3f9-48bd62522aa0)

4. Configuring and Building:
```
./configure
```
![Pasted image 20240417115212](https://github.com/lm3nitro/Projects/assets/55665256/37d0a7f1-ff5b-4cce-84d2-1df0d0ec80b9)
```
make
```
![Pasted image 20240417130023](https://github.com/lm3nitro/Projects/assets/55665256/76244837-a1c2-4949-8d99-06aa98eaadee)
```
make install
```
![Pasted image 20240417130133](https://github.com/lm3nitro/Projects/assets/55665256/f3191f99-a068-4609-9253-34aeb5ef85c8)

Note:

If the `configure` script fails, then it is most likely because it either couldn’t find a required dependency or it couldn’t find a sufficiently new version of a dependency. Assuming that you already installed all required dependencies, then you may need to use one of the `--with-*` options that can be given to the `configure` script to help it locate a dependency. To find out what all different options `./configure` supports, run `./configure --help`.

The default installation path is `/usr/local/zeek`, which would typically require root privileges when doing the `make install`. A different installation path can be chosen by specifying the `configure` script `--prefix` option. Note that `/usr`, `/opt/bro/`, and `/opt/zeek` are the standard prefixes for binary Zeek packages to be installed, so those are typically not good choices unless you are creating such a package.

5. Configuring the Run-Time Environment:

You may want to adjust your `PATH` environment variable according to the platform/shell/package you’re using since neither `/usr/local/zeek/bin/` nor `/opt/zeek/bin/` will reside in the default `PATH`. For example:

Bourne-Shell Syntax:
```
export PATH=/usr/local/zeek/bin:$PATH
```
![Pasted image 20240417130312](https://github.com/lm3nitro/Projects/assets/55665256/5816992b-57bd-47d2-bc63-1fe2864c9bb6)
```
nano .zshrc
```
![Pasted image 20240417130945](https://github.com/lm3nitro/Projects/assets/55665256/848df7db-6987-4404-a9b3-4525dc6c41f7)

Adding the path:

![Pasted image 20240417130812](https://github.com/lm3nitro/Projects/assets/55665256/983e1f38-6cd5-4995-b5b3-75937a94b4ee)

6. Verify Zeek:
![Pasted image 20240417130859](https://github.com/lm3nitro/Projects/assets/55665256/16a3b47b-da60-4fb4-ab42-16a5c94333e8)


# Installing Suricata:

![Screenshot 2024-04-27 at 10 49 33 PM](https://github.com/lm3nitro/Projects/assets/55665256/3b15223d-47be-4482-9eca-244aaf1aa396)

Next, I also wanted to install Suricata in my Ubuntu Server. These are the steps I used to get it install. 
Note:
On the "stable" version of Debian, Suricata is usually not available in the latest version. A more recent version is often available from Debian backports, if it can be built there.

1. To use backports, the backports repository for the current stable distribution needs to be added to the system-wide sources list. For Debian 10 (buster), for instance, run the following as root
```
echo "deb http://http.debian.net/debian buster-backports main" > \
    /etc/apt/sources.list.d/backports.list
apt-get update
apt-get install suricata -t buster-backports
```
![Pasted image 20240417132255](https://github.com/lm3nitro/Projects/assets/55665256/a4d3f3dc-b957-4f6f-ba13-4cbacae4feff)

![Pasted image 20240417132225](https://github.com/lm3nitro/Projects/assets/55665256/274ee7b1-fadd-460e-92bb-3e8c02179e4a)

2. Verify Suricata
```
suricata -h
```

![Pasted image 20240417132417](https://github.com/lm3nitro/Projects/assets/55665256/fe7bcd5d-cb72-41ea-aa07-987c13506161)

Suricata Commands: 

1. To start Suricata:
```
sudo systemctl start suricata
```
![Pasted image 20240417132655](https://github.com/lm3nitro/Projects/assets/55665256/797ea00f-d4f0-496d-a9fb-fc72fc6a1dc3)

2. To stop Suricata:
``` 
sudo systemctl stop suricata
```
![Pasted image 20240417132726](https://github.com/lm3nitro/Projects/assets/55665256/ea0e889a-dad6-4e66-829b-88c94deb9c4a)

3. To have Suricata start on-boot:
```
sudo systemctl enable suricata
```
4. To reload rules:
```
sudo systemctl reload suricata
```
5. Update Suricata
```
sudo suricata-update
```
![Pasted image 20240417141602](https://github.com/lm3nitro/Projects/assets/55665256/c53225e8-834b-4244-8077-d230b0471336)

### Summary

Now that I have RITA, ZEEK, and Suricata all running, I can start to do some threat hunting. Installing and configuring these tools expanded my knowledge in network security and threat detection, deepening my understanding of how various security technologies interoperate. This integration highlights the importance of interoperability among security systems, emphasizing how a layered security approach can enhance overall protection.

In the next section titled "Threat Hunting with RITA" I provide an example of this.  


