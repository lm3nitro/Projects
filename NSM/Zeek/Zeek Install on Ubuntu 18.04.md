# Install Zeek on Ubuntu 18.04

![Screenshot 2024-04-27 at 10 49 02 PM](https://github.com/lm3nitro/Projects/assets/55665256/ebae0b29-7a8f-4f23-86dc-409805c65628)

### SET MAX RING PARAMETERS

1. Use ethtool to determine the maximum ring parameters for your sniffing interfaces.  The example below assumes an interface named ens192.
    ```
    sudo ethtool -g ens192
    ```
2. As root/sudo, create a new file in /etc/networkd-dispatcher/routable.d/10-set-max-ring and add the following lines for each sniffing interface.
    ```
    #!/bin/sh
    # Set ring rx parameters for all sniffing interfaces
    ethtool -G ens192 rx 4096
    ```
3. Save the file and set its permissions to 755.
    ```
    sudo chmod 755 /etc/networkd-dispatcher/routable.d/10-set-max-ring
    ```
### DISABLE NIC OFFLOADING FUNCTIONS
 
1. As root/sudo, create a new file in /etc/networkd-dispatcher/routable.d/20-disable-checksum-offload and add the following lines for each sniffing interface.
   ```
   #!/bin/sh
   # Disable checksum offloading for all sniffing interfaces
   ethtool -K ens192 rx off tx off sg off tso off ufo off gso off gro off lro off
    ```
2. Save the file and set its permissions to 755.
    ```
    sudo chmod 755 /etc/networkd-dispatcher/routable.d/20-disable-checksum-offload
    ```
SET SNIFFING NETWORK INTERFACES TO PROMISCUOUS MODE

1. As root/sudo, create a new file in /etc/networkd-dispatcher/routable.d/30-enable-promisc-mode and add the following lines for each sniffing interface.
   ```
   #!/bin/sh
   # Enable promiscuous mode for all sniffing interfaces
   ip link set ens192 arp off multicast off allmulticast off promisc on
    ```
2. Save the file and set its permissions to 755.
    ```
    sudo chmod 755 /etc/networkd-dispatcher/routable.d/30-enable-promisc-mode
    ```
### CONFIRM CHANGES ARE PRESISTENT
  
1. Reboot your system and verify all the changes made thus far have persisted.Verify max ring parameters under Current hardware settings RX matches the configured maximum.
    ```
    sudo ethtool -g ens192
    ```
2. Verify NIC offloading features are turned off (this list is likely much longer on your system).
    ```
    sudo ethtool -k ens192
    ```
3. Verify that PROMISC is listed in the network interface status.
    ```
    ip address show ens192 | grep -i promisc
    ```
INSTALL ZEEK DEPENDENCIEs

1. Run the following apt command to download the required dependencies.
    ```
    sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python3 python3-dev python3-git python3-semantic-version swig zlib1g-dev libjemalloc-dev
    ```
2. Ensure all your packages are up to date and reboot your system.
    ```
    sudo apt-get update
    sudo apt-get dist-upgrade
    sudo reboot
    ```
### DOWNLOAD, COMPILE, AND INSTALL ZEEK
  
1. We will download zeek to the /home/zeek directory. Then we will configure Zeek to install in the /opt/zeek directory and enable jemalloc to improve memory and CPU usage.  As of this writing, the latest feature release is version 5.2.1.  If the download URL referenced in the wget command below no longer works, you can download the latest stable release directly from the Get Zeek download page.
    ```
    cd
    wget https://download.zeek.org/zeek-5.2.1.tar.gz
    tar -xzvf zeek-5.2.1.tar.gz
    cd zeek-5.2.1
    ./configure --prefix=/opt/zeek --enable-jemalloc --build-type=release
     ```
>### Note: Use the following if you get an error using the make command in regards to cmake and g++ not on the correct version:

```
    g++ --version
    sudo apt install build-essential
    sudo apt -y install g++-7 g++-8 g++-9 g++-10
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 7
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 9
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 10
    sudo update-alternatives --config g++
    sudo apt -y install gcc-7 g++-7 gcc-8 g++-8 gcc-9 g++-9
    sudo update-alternatives --config gcc
    sudo apt purge --auto-remove cmake
    wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | 
    sudo tee/etc/apt/trusted.gpg.d/kitware.gpg >/dev/null
    sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'
    sudo apt update
    sudo apt install cmake
    make
    make install
 ```
    
>### Note: This will take *a while* to compile.

2. Switch back to your normal user by closing the zeek session.
    ```
    exit
    ```
3. Since the zeek user is not root, give the Zeek binaries permissions to capture packets.
    ```
    sudo setcap cap_net_raw=eip /opt/zeek/bin/zeek
    sudo setcap cap_net_raw=eip /opt/zeek/bin/capstats
    ```

### ADD ZEEK TO PATH

1. Switch back to the zeek user.
    ```
    su zeek
    ```
2. As the zeek user, create ~/.bashrc and add the following lines.
    ```
    #Add Zeek to PATH
    export PATH="/opt/zeek/bin:$PATH"
    ```
4. Save the file and apply the new path to the zeek user.
    ```
    source ~/.bashrc
    ```
### CONFIGURE ZEEK

1. Edit /opt/zeek/etc/node.cfg to configure the number of nodes. It is recommended to use a maximum of one or two less workers than the total number of CPU cores available on your sensor. In the example configuration below we are configuring a total of two workers, analyzing one sniffing interface.
 
>### Note: The following node configuration does not use Zeek’s out of the box support for AF_PACKET (as of version 5.2). It is recommended to configure Zeek to use AF_PACKET for optimal packet capture and the configuration is covered in Part II.

```
    # Example ZeekControl node configuration.
    # Below is an example clustered configuration on a single host.
    [logger]
    type=logger
    host=localhost
    [manager]
    type=manager
    host=localhost
    [proxy-1]
    type=proxy
    host=localhost
    [worker-1]
    type=worker
    host=localhost
    interface=ens192
    [worker-2]
    type=worker
    host=localhost
    interface=ens192
 ```
>### In the event you have two or more sniffing interfaces (e.g. enp2s0 and enp3s0), see the example configuration below which assigns each interface its own worker.
   
```
    # Example ZeekControl node configuration.
    # Below is an example clustered configuration on a single host.
    [logger]
    type=logger
    host=localhost
    [manager]
    type=manager
    host=localhost
    [proxy-1]
    type=proxy
    host=localhost
    [worker-1]
    type=worker
    host=localhost
    interface=ens192
    [worker-2]
    type=worker
    host=localhost
    interface=ens1192
```
2. Edit /opt/zeek/share/zeek/site/local.zeek to enable or disable scripts as needed.
    ```
    start zeek
    ```
3. As the zeek user, run zeekctl deploy to apply configurations and run Zeek.
    ```
    zeekctl deploy
    ```

### ZEEKCONTROL CRON

>### ZeekControl features a cron command to check for and restart crashed nodes and to perform other maintenance tasks. To take advantage of this, let’s set up a cron job.

1. Edit the crontab of the non-root zeek user.
    ```
    crontab -e
    ```
2. Add the following to set up a cron job that runs every five minutes.  You can adjust the frequency to your liking.
    ```
    */5 * * * * /opt/zeek/bin/zeekctl cron
    ```

### SET UP ZEEK PACKAGE MANAGER

1. As the zeek user, make sure you’re in its respective home directory.
    ```
    cd
    ```
2. As the zeek user, install zkg’s dependencies. This will install two external Python modules that zkg requires to ~/.local/lib/python3.6/site-packages.
    ```
    pip3 install GitPython semantic-version --user
    ```
3. As the zeek user, configure Zeek Package Manager (zkg).
    ```
    zkg autoconfig
    ```
>### This will create a configuration file in /opt/zeek/etc/zkg/config. Upon completion it should look something like the following.

 ```
    zeek = https://github.com/zeek/packages
    [paths]
    state_dir = /opt/zeek/var/lib/zkg
    script_dir = /opt/zeek/share/zeek/site
    plugin_dir = /opt/zeek/lib64/zeek/plugins
    zeek_dist = /home/zeek/zeek-5.2.1
  ```
4. If zkg is not installed or executed properly, you may see the following error:
    ```
    zkg
    error: zkg failed to import one or more dependencies:
    * GitPython:        https://pypi.org/project/GitPython
    * semantic-version: https://pypi.org/project/semantic-version
    ```
>### If you use 'pip', they can be installed like:
    ```
    pip3 install GitPython semantic-version
    ```
### CONFIGURE ZEEK TO USE AF_PACKET

1. Edit /opt/zeek/etc/node.cfg to configure Zeek to use AF_PACKET.  In the example configuration below we are configuring one worker, load balanced across two cores, analyzing one sniffing interface.
    ```
    # Example ZeekControl node configuration.
    # Below is an example clustered configuration on a single host.
    [logger]
    type=logger
    host=localhost
    [manager]
    type=manager
    host=localhost
    [proxy-1]
    type=proxy
    host=localhost
    [worker-1]
    type=worker
    host=localhost
    interface=af_packet::ens192
    lb_method=custom
    lb_procs=2
    pin_cpus=0,1
    ```
>### In the event you have two or more sniffing interfaces (e.g. enp2s0 and enp3s0), see the example configuration below which assigns each interface its own worker, load balanced across two cores, again using AF_PACKET. Note the addition of unique af_packet_fanout_id values for each of the sniffing interfaces.
  
```
   # Example ZeekControl node configuration.
   # Below is an example clustered configuration on a single host.
    [logger]
    type=logger
    host=localhost
    [manager]
    type=manager
    host=localhost
    [proxy-1]
    type=proxy
    host=localhost
    [worker-1]
    type=worker
    host=localhost
    interface=af_packet::ens192
    lb_method=custom
    lb_procs=2
    pin_cpus=0,1
    af_packet_fanout_id=2
    [worker-2]
    type=worker
    host=localhost
    interface=af_packet::ens192
    lb_method=custom
    lb_procs=2
    pin_cpus=2,3
    af_packet_fanout_id=3
 ```
2. As root/sudo, give the Zeek binaries permissions to capture packets. This was previously done in Part I, however, installing AF_PACKET requires doing this again.
    ```
    sudo setcap cap_net_raw=eip /opt/zeek/bin/zeek
    sudo setcap cap_net_raw=eip /opt/zeek/bin/capstats
    ```
3. As the zeek user, run zeekctl deploy to apply configurations and run Zeek.
    ```
    zeekctl deploy
    ```

### INSTALL ADDITIONAL USEFUL PACKAGES (E.G. ADD-INTERFACES, JA3, AND HASSH)

>### We’ll install additional Zeek packages: add-interfaces, ja3, and HASSH. The install process outlined below should work for installing other packages you may be interested in.

1. As the zeek user, stop Zeek if it is currently running.
    ```
    zeekctl stop
    ```
2. Use zkg to install the add-interfaces package. In situations where you are monitoring multiple network interfaces on one sensor, this adds an “_interface” field to every log file which labels the particular network interface that traffic is coming from.
    ```
    zkg install zeek/j-gras/add-interfaces
    The following packages will be INSTALLED:
    zeek/j-gras/add-interfaces (master)
    Proceed? [Y/n] y
    Installed "zeek/j-gras/add-interfaces" (master)
    Loaded "zeek/j-gras/add-interfaces"
    ```
3. Edit /opt/zeek/share/zeek/site/add-interfaces/add-interfaces.bro and modify the const enable_all_logs and const include_logs:
    ```
    set[Log::ID] fields as shown below. Save the file when you’re finished.
    export {
        ## Enables interfaces for all active streams
        const enable_all_logs = T &redef;
        ## Streams not to add interfaces for
        const exclude_logs: set[Log::ID] = { } &redef;
        ## Streams to add interfaces for
        const include_logs: set[Log::ID] = { } &redef;
        }
    ```
4. Use zkg to install the ja3 package. This is used for profiling SSL/TLS clients.
    ```
    zkg install zeek/salesforce/ja3
    The following packages will be INSTALLED:
    zeek/salesforce/ja3 (master)
    Proceed? [Y/n] y
    Installing "zeek/salesforce/ja3"
    Installed "zeek/salesforce/ja3" (master)
    Loaded "zeek/salesforce/ja3"
    ```
5. Use zkg to install the HASSH package. This is used for profiling SSH clients and servers.
    ```
    zkg install zeek/salesforce/hassh
    The following packages will be INSTALLED:
    zeek/salesforce/hassh (master)
    Proceed? [Y/n] y
    Installing "zeek/salesforce/hassh"
    Installed "zeek/salesforce/hassh" (master)
    Loaded "zeek/salesforce/hassh"
    ```
6. Edit /opt/zeek/share/zeek/site/local.zeek and add the following lines to the bottom. This will load all packages you’ve installed.
    ```
    # Load Zeek Packages
    @load packages
    ```
7. As the zeek user, run zeekctl deploy to apply configurations and run Zeek.
    ```
    zeekctl deploy
    ```

### UPDATE INSTALLED ZEEK PACKAGES
1. As the zeek user, stop Zeek if it is currently running.
    ```
    zeekctl stop
    ```
2. Use zkg to check for updated packages.
   ```
   zkg refresh
   Refresh package source: zeek
   No changes
   Refresh installed packages
   New outdated packages:
   zeek/salesforce/hassh (master)
   ```
>### This indicates that zeek/salesforce/hassh needs to be updated.

3. Use zkg to check for updated packages.
    ```
    zkg upgrade
    The following packages will be UPGRADED:
    zeek/salesforce/hassh (master)
    Proceed? [Y/n] y
    Upgraded "zeek/salesforce/hassh" (master)
    ```
4. As the zeek user, run zeekctl deploy to apply configurations and run Zeek.
    ```
    zeekctl deploy
    ```

### ENABLE FILE HASHING AND TEAM CYMRU’S MALWARE HASH REGISTRY LOOKUPS

1. By default, automatic file hashing and Team Cymru’s Malware Hash Registry lookups are enabled. To confirm this, open /opt/zeek/share/zeek/site/local.zeek and look for the following lines. Ensure they appear as below and the @load lines are not commented out (e.g., do not have a # symbol in front). Update the file if needed.
    
<img width="433" alt="Screenshot 2024-03-24 at 1 15 56 PM" src="https://github.com/lm3nitro/Zeek/assets/55665256/59d05b04-9928-4539-9d04-b9716aabc010">


2. SHA256 hashing is not enabled by default.  We will enable this by creating a simple Zeek script.  As the zeek user, create a new file /opt/zeek/share/zeek/site/hash_sha256.zeek, add the following lines, and then save the file.
    ```
  	##! Perform SHA256 hashing on all files.
    @load base/files/hash
    event file_new(f: fa_file)
        {
    Files::add_analyzer(f, Files::ANALYZER_SHA256);
        }
    ```
3. As the zeek user, edit /opt/zeek/share/zeek/site/local.zeek, add the following lines, and then save the file.
    ```
    # Add SHA256 hash for files
    @load hash_sha256
    ```
  <img width="677" alt="Screenshot 2024-03-24 at 1 01 39 PM" src="https://github.com/lm3nitro/Zeek/assets/55665256/f26a5379-1429-44eb-b604-211f4620c449">
  
4.	As the zeek user, stop zeek.
    ```
    zeekctl stop
    ```
5. As the zeek user, apply the new settings and start zeek.
    ```
    zeekctl deploy
    ENABLE AUTOMATIC FILE EXTRACTION
    ```
6. As the zeek user, stop Zeek if it is currently running.
    ```
    zeekctl stop
    ```
7. Use zkg to install the file extraction package.
    ```
    zkg install zeek/hosom/file-extraction
    The following packages will be INSTALLED:
    zeek/hosom/file-extraction (2.0.3)
    Proceed? [Y/n] y
    Installing "zeek/hosom/file-extraction".
    Installed "zeek/hosom/file-extraction" (2.0.3)
    Loaded "zeek/hosom/file-extraction"
    ```
8. Configure file extraction options by editing /opt/zeek/share/zeek/site/file-extraction/config.zeek. Below is a sample config.zeek that will set the directory to store extracted files to /opt/zeek/extracted/ and set the files we want to automatically extract to commonly exploited file types (e.g., Java, PE, Microsoft Office, and PDF).

  <img width="387" alt="Screenshot 2024-03-24 at 1 12 11 PM" src="https://github.com/lm3nitro/Zeek/assets/55665256/199b205b-e5d6-4456-a68c-cccb7c36d4b4">

9. Create the directory to save all extracted files. It must match what we set in config.zeek.
    ```
    mkdir /opt/zeek/extracted
    ```
10. If this is your first time installing a Zeek package, edit /opt/zeek/share/zeek/site/local.zeek and add the following lines to the bottom. This will load all packages you’ve installed. You will only need to do this once.
    ```
    # Load Zeek Packages
    @load packages
    ```
11. As the zeek user, apply the new settings and start zeek.
    ```
    zeekctl deploy
    ```
    

    
