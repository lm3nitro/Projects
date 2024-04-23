
# Database Installation :

# Import the public key used by the package management system:

sudo apt-get install gnupg

![Pasted image 20240417111515](https://github.com/lm3nitro/Projects/assets/55665256/bd445abe-36b1-4685-b3e2-4f77eb809380)

# Import the MongoDB public GPG Key:

curl -fsSL https://pgp.mongodb.com/server-4.2.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-4.2.gpg \
   --dearmor

![Pasted image 20240417112228](https://github.com/lm3nitro/Projects/assets/55665256/5393d35e-f6fb-4867-9ed2-650d10061a81)

# Create a /etc/apt/sources.list.d/mongodb-org-4.2.list file for MongoDB :

echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-4.2.gpg ] http://repo.mongodb.org/apt/debian buster/mongodb-org/4.2 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list


![Pasted image 20240417112240](https://github.com/lm3nitro/Projects/assets/55665256/34687981-c111-4eb8-a3b8-91fdfe409393)

# Reload local package database:

sudo apt-get update


![Pasted image 20240417112253](https://github.com/lm3nitro/Projects/assets/55665256/30ba1278-db34-4354-99b3-a72c2208e64b)


# Install the MongoDB packages:

sudo apt-get install -y mongodb-org


![Pasted image 20240417112306](https://github.com/lm3nitro/Projects/assets/55665256/95ed4fdd-8d53-4c15-ab02-a13d6398f983)



# Start MongoDB:

sudo systemctl start mongod

![Pasted image 20240417112719](https://github.com/lm3nitro/Projects/assets/55665256/91983b05-12dc-4d0c-b7cc-f982bec6a9c9)

Note:
If you receive an error similar to the following when starting mongod:
Failed to start mongod.service: Unit mongod.service not found.

Run the following command first:

If you receive an error similar to the following when starting mongod:
Failed to start mongod.service: Unit mongod.service not found.

Run the following command first:

# Verify that MongoDB has started successfully:



sudo systemctl status mongod

![Pasted image 20240417112840](https://github.com/lm3nitro/Projects/assets/55665256/656219c2-dcb9-46e2-9324-d3aea55067a8)


Note:
You can optionally ensure that MongoDB will start following a system reboot by issuing the following command:

sudo systemctl enable mongod

![Pasted image 20240417112906](https://github.com/lm3nitro/Projects/assets/55665256/3cef96ae-13ee-4e5e-965e-c88de89757cd)


# Stop MongoDB:

As needed, you can stop the mongod process by issuing the following command:

sudo systemctl stop mongod


# Restart MongoDB.

You can restart the mongod process by issuing the following command:


sudo systemctl restart mongod



# Rita Building from Source :



Installing Golang

In order to compile RITA manually you will need to install Golang (v1.13 or greater).
Building RITA


```
# First, install the package
sudo apt install -y golang

# Then add the following to your .bashrc
export GOROOT=/usr/lib/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

# Reload your .bashrc
source .bashrc
```

Logs:

![Pasted image 20240417113744](https://github.com/lm3nitro/Projects/assets/55665256/c4654717-f1a3-43d1-9092-e0bdb4d8d19a)


Building RITA

At this point you can build RITA from source code.

    git clone https://github.com/activecm/rita.git
    cd rita
    make (Note that you will need to have make installed. You can use your system's package manager to install it.)


![Pasted image 20240417113130](https://github.com/lm3nitro/Projects/assets/55665256/f1f27727-cde3-46bb-ba57-366c67128b8c)

![Pasted image 20240417113206](https://github.com/lm3nitro/Projects/assets/55665256/f8fbbe0c-1ed8-4510-bdd1-775dc1367d35)


Configuring the system

RITA requires a few directories to be created for it to function correctly.

    sudo mkdir /etc/rita && sudo chmod 755 /etc/rita
    sudo mkdir -p /var/lib/rita/logs && sudo chmod -R 755 /var/lib/rita
    sudo chmod 777 /var/lib/rita/logs

Copy the config file from your local RITA source code.

    sudo cp etc/rita.yaml /etc/rita/config.yaml && sudo chmod 666 /etc/rita/config.yaml

![Pasted image 20240417113315](https://github.com/lm3nitro/Projects/assets/55665256/73231609-c7b0-46d0-a40b-78ec4e148215)

At this point, you can modify the config file as needed and test using the rita test-config command.

# Verify Rita Version:

![Pasted image 20240417150321](https://github.com/lm3nitro/Projects/assets/55665256/4847afcf-0300-44b0-9b22-dee8584bad7c)

# Note:

RITA itself only puts files in a few places: `/usr/local/bin/rita`, `/etc/rita/`, and `/var/lib/rita/`.  If you want remove Rita, you can just directly delete all those.

## Upgrading Between Minor or Patch Versions :

If you are upgrading within the same major version (e.g. v2.0.0 to v2.0.1, or v3.0.0 to v3.1.0) all you need to do is download the newest RITA binary and replace the one on your system.


Example: 

Replace the binary located `/usr/local/bin/rita`  with the new binary.

![Pasted image 20240419110545](https://github.com/lm3nitro/Projects/assets/55665256/1aefe3e4-495b-4209-83a4-1f9af926ec46)



# Zeek  Building from Source:


### Required Dependencies:


- sudo apt-get install cmake make gcc g++ flex libfl-dev bison libpcap-dev libssl-dev python3 python3-dev swig zlib1g-dev

![Pasted image 20240417114644](https://github.com/lm3nitro/Projects/assets/55665256/c56054f7-e416-456d-aa02-46521b037a4a)

### Optional Dependencies:


- sudo apt-get install python3-git python3-semantic-version

![Pasted image 20240417114721](https://github.com/lm3nitro/Projects/assets/55665256/79e4704a-2af2-46ae-87ec-07b60982af60)


#### Retrieving the Sources:

git clone --recurse-submodules https://github.com/zeek/zeek

![Pasted image 20240417114851](https://github.com/lm3nitro/Projects/assets/55665256/15dc6bda-d48f-481c-b3f9-48bd62522aa0)


#### Configuring and Building:

./configure

![Pasted image 20240417115212](https://github.com/lm3nitro/Projects/assets/55665256/37d0a7f1-ff5b-4cce-84d2-1df0d0ec80b9)


make

![Pasted image 20240417130023](https://github.com/lm3nitro/Projects/assets/55665256/76244837-a1c2-4949-8d99-06aa98eaadee)



make install

![Pasted image 20240417130133](https://github.com/lm3nitro/Projects/assets/55665256/f3191f99-a068-4609-9253-34aeb5ef85c8)




Note:

If the `configure` script fails, then it is most likely because it either couldn’t find a required dependency or it couldn’t find a sufficiently new version of a dependency. Assuming that you already installed all required dependencies, then you may need to use one of the `--with-*` options that can be given to the `configure` script to help it locate a dependency. To find out what all different options `./configure` supports, run `./configure --help`.

The default installation path is `/usr/local/zeek`, which would typically require root privileges when doing the `make install`. A different installation path can be chosen by specifying the `configure` script `--prefix` option. Note that `/usr`, `/opt/bro/`, and `/opt/zeek` are the standard prefixes for binary Zeek packages to be installed, so those are typically not good choices unless you are creating such a package.


## Configuring the Run-Time Environment:

You may want to adjust your `PATH` environment variable according to the platform/shell/package you’re using since neither `/usr/local/zeek/bin/` nor `/opt/zeek/bin/` will reside in the default `PATH`. For example:

Bourne-Shell Syntax:

export PATH=/usr/local/zeek/bin:$PATH

![Pasted image 20240417130312](https://github.com/lm3nitro/Projects/assets/55665256/5816992b-57bd-47d2-bc63-1fe2864c9bb6)


nano .zshrc

![Pasted image 20240417130945](https://github.com/lm3nitro/Projects/assets/55665256/848df7db-6987-4404-a9b3-4525dc6c41f7)


Adding the path:

![Pasted image 20240417130812](https://github.com/lm3nitro/Projects/assets/55665256/983e1f38-6cd5-4995-b5b3-75937a94b4ee)



# Verify Zeek:


![Pasted image 20240417130859](https://github.com/lm3nitro/Projects/assets/55665256/16a3b47b-da60-4fb4-ab42-16a5c94333e8)



# Installing Suricata:


n the "stable" version of Debian, Suricata is usually not available in the latest version. A more recent version is often available from Debian backports, if it can be built there.

To use backports, the backports repository for the current stable distribution needs to be added to the system-wide sources list. For Debian 10 (buster), for instance, run the following as root


echo "deb http://http.debian.net/debian buster-backports main" > \
    /etc/apt/sources.list.d/backports.list


apt-get update
apt-get install suricata -t buster-backports

![Pasted image 20240417132225](https://github.com/lm3nitro/Projects/assets/55665256/46c7a75b-62b6-4831-bbdf-a3193574e5a8)


![Pasted image 20240417132225](https://github.com/lm3nitro/Projects/assets/55665256/274ee7b1-fadd-460e-92bb-3e8c02179e4a)


# Verify Suricata:

![Pasted image 20240417132417](https://github.com/lm3nitro/Projects/assets/55665256/fe7bcd5d-cb72-41ea-aa07-987c13506161)




# To start Suricata:

sudo systemctl start suricata

![Pasted image 20240417132655](https://github.com/lm3nitro/Projects/assets/55665256/797ea00f-d4f0-496d-a9fb-fc72fc6a1dc3)


# To stop Suricata:

sudo systemctl stop suricata

![Pasted image 20240417132726](https://github.com/lm3nitro/Projects/assets/55665256/ea0e889a-dad6-4e66-829b-88c94deb9c4a)


# To have Suricata start on-boot:

sudo systemctl enable suricata

# To reload rules:

sudo systemctl reload suricata



# Updating Suricata:

![Pasted image 20240417141602](https://github.com/lm3nitro/Projects/assets/55665256/c53225e8-834b-4244-8077-d230b0471336)


