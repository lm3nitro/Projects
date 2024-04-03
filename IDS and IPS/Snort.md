# Snort 3


INSTALL PRE-REQS


sudo apt-get install -y build-essential autotools-dev libdumbnet-dev libluajit-5.1-dev libpcap-dev \ zlib1g-dev pkg-config libhwloc-dev cmake liblzma-dev openssl libssl-dev cpputest libsqlite3-dev \ libtool uuid-dev git autoconf bison flex libcmocka-dev libnetfilter-queue-dev libunwind-dev \ libmnl-dev ethtool libjemalloc-dev

![Pasted image 20240402161505](https://github.com/lm3nitro/Projects/assets/55665256/f8ec5aee-0ecc-4968-8095-f67ce10e5ec2)


STEP 1 – Install pcre
cd ~/snort 
wget https://sourceforge.net/projects/pcre/files/pcre/8.45/pcre-8.45.tar.gz 
tar -xzvf pcre-8.45.tar.gz 
cd pcre-8.45
./configure 
make 
sudo make install

![Pasted image 20240402161837](https://github.com/lm3nitro/Projects/assets/55665256/678a3705-4cba-4c8e-8006-4d0d929682f5)

![Pasted image 20240402161907](https://github.com/lm3nitro/Projects/assets/55665256/7b4125ba-801a-4241-b795-348047822037)

![Pasted image 20240402161937](https://github.com/lm3nitro/Projects/assets/55665256/6c7c9a7e-4ebf-4fd3-90f6-f6b195ca5697)

![Pasted image 20240402162004](https://github.com/lm3nitro/Projects/assets/55665256/b0ffddb9-5c6b-4545-9e33-75d9eac433f7)



STEP 3 – Install gperftools
cd ~/snort 
wget https://github.com/gperftools/gperftools/releases/download/gperftools-2.9.1/gperftools-2.9.1.tar.gz 
tar xzvf gperftools-2.9.1.tar.gz 
cd gperftools-2.9.1 
./configure 
make 
sudo make install


![Pasted image 20240402162602](https://github.com/lm3nitro/Projects/assets/55665256/8249d562-7979-4f5c-a0d2-75ceff5cea22)


STEP 4 – Install Ragel
cd ~/snort 
wget http://www.colm.net/files/ragel/ragel-6.10.tar.gz 
tar -xzvf ragel-6.10.tar.gz 
cd ragel-6.10 
./configure 
make 
sudo make install

![Pasted image 20240402162848](https://github.com/lm3nitro/Projects/assets/55665256/07e9b55e-22cd-4e0e-8d75-c28150b04a21)


STEP 5 - download (but don’t install) the Boost C++ Libraries:
cd ~/snort 
wget https://boostorg.jfrog.io/artifactory/main/release/1.77.0/source/boost_1_77_0.tar.gz 
tar -xvzf boost_1_77_0.tar.gz





STEP 6 – Install Hyperscan
cd ~/snort 
wget https://github.com/intel/hyperscan/archive/refs/tags/v5.4.2.tar.gz 
tar -xvzf v5.4.2.tar.gz 
mkdir ~/snort/hyperscan-5.4.2-build 
cd hyperscan-5.4.2-build/ 
cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DBOOST_ROOT=~/snort/boost_1_77_0/ ../hyperscan-5.4.2 
make 
sudo make install

![Pasted image 20240402163307](https://github.com/lm3nitro/Projects/assets/55665256/e6255a84-c57c-4f52-b675-82eb5fe175e8)

![Pasted image 20240402164013](https://github.com/lm3nitro/Projects/assets/55665256/104c3b78-b731-4665-b17e-70282133e21c)

![Pasted image 20240402164050](https://github.com/lm3nitro/Projects/assets/55665256/58116954-cf70-4097-b770-1da280f87c96)

![Pasted image 20240402165315](https://github.com/lm3nitro/Projects/assets/55665256/95d8714f-cd5e-4a9c-8614-30f0bc5b2de2)

![Pasted image 20240402165345](https://github.com/lm3nitro/Projects/assets/55665256/414b5aa5-e7c5-42b8-ab1e-d00d05c84bc4)


STEP 7 – Install flatbuffers
cd ~/snort 
wget https://github.com/google/flatbuffers/archive/refs/tags/v2.0.0.tar.gz -O flatbuffers-v2.0.0.tar.gz 
tar -xzvf flatbuffers-v2.0.0.tar.gz 
mkdir flatbuffers-build 
cd flatbuffers-build 
cmake ../flatbuffers-2.0.0 
make 
sudo make install

![Pasted image 20240402165605](https://github.com/lm3nitro/Projects/assets/55665256/7beb708d-b5fc-4efd-8946-c4baea66e39f)

![Pasted image 20240402165624](https://github.com/lm3nitro/Projects/assets/55665256/56161d1f-c370-4734-88bb-ea93c238bcc2)


STEP 8 - Install Data Acquistion (DAQ) from Snort
cd ~/snort 
wget https://github.com/snort3/libdaq/archive/refs/tags/v3.0.13.tar.gz -O libdaq-3.0.13.tar.gz
tar -xzvf libdaq-3.0.13.tar.gz 
cd libdaq-3.0.13 
./bootstrap 
./configure 
make 
sudo make install

STEP 9 - Update shared libraries
sudo ldconfig

![Pasted image 20240402165754](https://github.com/lm3nitro/Projects/assets/55665256/e09460d8-68cc-443a-a302-a85ba32b9a62)

![Pasted image 20240402165840](https://github.com/lm3nitro/Projects/assets/55665256/d0cd33be-3a41-48b2-aaf2-a7c3d6dc640e)

![Pasted image 20240402165927](https://github.com/lm3nitro/Projects/assets/55665256/0876f3fe-149f-483e-af6b-bdd8008786a4)

STEP 10 – Download latest version of Snort 3
cd ~/snort 
wget https://github.com/snort3/snort3/archive/refs/tags/3.1.74.0.tar.gz -O snort3-3.1.74.0.tar.gz 
tar -xzvf snort3-3.1.74.0.tar.gz 
cd snort3-3.1.74.0 
./configure_cmake.sh --prefix=/usr/local --enable-jemalloc 
cd build 
make 
sudo make install
![Pasted image 20240402170115](https://github.com/lm3nitro/Projects/assets/55665256/cb9b28ef-6203-4f0f-ac17-d6b58419a1e4)
![Pasted image 20240402170138](https://github.com/lm3nitro/Projects/assets/55665256/fcf89b37-623c-4aa9-a9b3-29697945392f)
![Pasted image 20240402170208](https://github.com/lm3nitro/Projects/assets/55665256/8ebeb2aa-40fe-4171-a07c-d02cc6227db9)
![Pasted image 20240402171429](https://github.com/lm3nitro/Projects/assets/55665256/054fcfb0-9332-490e-8f9a-d91d21332f33)
![Pasted image 20240402171652](https://github.com/lm3nitro/Projects/assets/55665256/6afff320-5c53-4cd4-9980-98b38d3ac6a5)



# Verify snort 3 is working or running


Note: Snort 3 binary is on /usr/local/bin

![Pasted image 20240402175208](https://github.com/lm3nitro/Projects/assets/55665256/7d24a925-6d94-4145-8526-fe19ae949ed2)

![Pasted image 20240402175104](https://github.com/lm3nitro/Projects/assets/55665256/2fe6afaf-c2ec-4ac1-a959-3bf594f2d261)

# Testing the Snort 3 configuration file:

Note: The configuration file is located at  /usr/local/etc/snort/snort.lua

 snort -c /usr/local/etc/snort/snort.lua
![Pasted image 20240402175506](https://github.com/lm3nitro/Projects/assets/55665256/d7b09f9c-a4da-482d-a29b-6b7cdc80f8b2)

# Optional 1:

Note: If want to run snort to sniff traffic live on the interface, so it can generate alert in real time, there are a few things that need to be setup before hand.

Verify the name of the interface:

![Pasted image 20240402175706](https://github.com/lm3nitro/Projects/assets/55665256/51add544-dfac-421c-9ea4-fb6f1480e949)


# Verify receive offload on the interface:

Note:

Some NIC/driver combinations may disable these offload settings by default, while others enable it by default.  You should check your sensors now before you get into a situation where you really need that full packet capture and find out that you don't actually have it.  To check, run ethtool with the "-k" (lower-case k) option on the interface you'd like to check.  For example, to check ens32:

![Pasted image 20240402175935](https://github.com/lm3nitro/Projects/assets/55665256/10a51022-9b49-4b81-bb3e-c747f1b7ffb8)

# Temporary disable interface offload:

Note: You can set multiple options in one "ethtool" command, but this can be problematic if your card doesn't support all of the settings.  To avoid this, you could invoke ethtool for each option like this:

ethtool -K ens32 rx off  
ethtool -K ens32 tx off  
ethtool -K ens32 sg off  
ethtool -K ens32 tso off  
ethtool -K ens32 ufo off  
ethtool -K ens32 gso off  
ethtool -K ens32 gro off  
ethtool -K ens32 lro off



Or we could simply wrap the ethtool command in a for-loop like this:

for i in rx tx sg tso ufo gso gro lro; do ethtool -K ens32 $i off; done

![Pasted image 20240402180832](https://github.com/lm3nitro/Projects/assets/55665256/847b1ccd-dba7-4cc5-acdb-a684537594c6)

Note:
These settings will remain in effect only while the OS is booted, so this needs to be applied at every boot.  This can be done by adding the above for-loop as a "post-up" command for each of the interfaces in /etc/network/interfaces.



# Creating a custom rule on Snort 3:


Node:  We need to create a directory call rules under /usr/local/etc/ and  create a file call loca.rules under /usr/local/etc/rules/. This is where we are going create our custom rule.


mkdir /usr/local/etc/rules
nano /usr/local/etc/rules/local.rules

![Pasted image 20240402183512](https://github.com/lm3nitro/Projects/assets/55665256/0caec5a0-848c-4490-8704-2d6f199cdb32)

Running  the rules against the configuration file:

snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/rules/local.rules

![Pasted image 20240402183701](https://github.com/lm3nitro/Projects/assets/55665256/310e4699-f28a-4392-85a9-36fa0a106e5a)



Running Snort on the interface for rule testing:

![Pasted image 20240402184234](https://github.com/lm3nitro/Projects/assets/55665256/4a3747fe-9cd6-4099-b12f-d0e17c376986)


snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/rules/local.rules -i ens32 -A alert_fast

![Pasted image 20240402184126](https://github.com/lm3nitro/Projects/assets/55665256/e6d712d4-08d5-49d9-83e4-63b4781a1f2b)

Configure Snort 3 configuration file to make the local.rules persistent that way we don't have to specify the location of our local rules.

Remove -- to enable the option:

 -- enable_builtin_rules = true,

Adding:
 include= "/usr/local/etc/rules/local.rules", 

Save the configuration file and test:

![Pasted image 20240402184804](https://github.com/lm3nitro/Projects/assets/55665256/1b8e391e-25a6-4972-b88c-a3bacb753c7b)




Running  the configuration file again wit out specifying the location of the local.rules :

![Pasted image 20240402185426](https://github.com/lm3nitro/Projects/assets/55665256/e5d0e28c-5b4c-4b43-8598-299a980857bb)


snort -c /usr/local/etc/snort/snort.lua -i ens32 -A alert_fast

![Pasted image 20240402185237](https://github.com/lm3nitro/Projects/assets/55665256/4fc959cb-e7ca-4ed1-9a4e-07d012722b93)

# Running Snort against a pcap file:

 snort -c /usr/local/etc/snort/snort.lua -r icmp.pcap -A alert_fast -q

![Pasted image 20240402185922](https://github.com/lm3nitro/Projects/assets/55665256/e2cc0fd8-aa72-439f-b3d6-637571c3dcf5)

# Permanent disable interface offload:

Disable LRO & GRO using a service

Note: Create a file under /bin/systemd/system/

![Pasted image 20240402181318](https://github.com/lm3nitro/Projects/assets/55665256/167a82d7-2b1b-4051-a5a5-d2e4ac707647)

Enable the service:

![Pasted image 20240402181246](https://github.com/lm3nitro/Projects/assets/55665256/a9604909-1a2e-460c-a7d2-4e5b7d625f8f)

####
[Unit] 
Description=Ethtool Configration for Network Interface 
[Service] 
Requires=network.target 
Type=oneshot 
ExecStart=/sbin/ethtool -K <network adapter> gro off 
ExecStart=/sbin/ethtool -K <network adapter> lro off 
[Install] 
WantedBy=multi-user.target

###


Optional 2:

PulledPork is **an opensource perl script that can automatically update Snort rules**.

git clone https://github.com/shirkdog/pulledpork3.git 
cd ~/snort/pulledpork3 
sudo mkdir /usr/local/bin/pulledpork3 
sudo cp pulledpork.py /usr/local/bin/pulledpork3 
sudo cp -r lib/ /usr/local/bin/pulledpork3 
sudo chmod +x /usr/local/bin/pulledpork3/pulledpork.py 
sudo mkdir /usr/local/etc/pulledpork3 
sudo cp etc/pulledpork.conf /usr/local/etc/pulledpork3/
