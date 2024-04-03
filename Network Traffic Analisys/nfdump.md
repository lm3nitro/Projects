nfdump


netflow display and analyzing program of the nfdump tool set. It reads the netflow data from files stored by nfcapd and processes the flows according the options given. The filter syntax is comparable to tcpdump and extended for netflow data. Nfdump can also display many different top N flow and flow element statistics.


# List Benefices for threat hunting and incidents response: 



# Dependencies:

apt install make gcc flex rrdtool librrd-dev libpcap-dev php librrds-perl libsocket6-perl apache2 libapache2-mod-php libtool dh-autoreconf pkg-config libbz2-dev byacc doxygen graphviz librrdp-perl libmailtools-perl build-essential autoconf

![Pasted image 20240402011608](https://github.com/lm3nitro/Projects/assets/55665256/3be383d0-f17a-4ce8-84db-07f4717ced61)

wget https://github.com/phaag/nfdump/archive/v1.6.23.tar.gz

![Pasted image 20240402011755](https://github.com/lm3nitro/Projects/assets/55665256/5fa026b7-c2df-4af8-a29a-894cec1f0c8e)


 tar xvzf nfdump-1.6.23.tar.gz

![Pasted image 20240402011851](https://github.com/lm3nitro/Projects/assets/55665256/3f4f0c07-a74b-45bb-9aa7-46306bf4872f)

cd nfdump-1.6.23/  
sh ./autogen.sh  

![Pasted image 20240402013240](https://github.com/lm3nitro/Projects/assets/55665256/1e642a35-6516-41b0-aebf-0493b1240553)


./configure --enable-nsel --enable-nfprofile --enable-sflow --enable-readpcap --enable-nfpcapd --enable-nftrack  



make && make install

![Pasted image 20240402013454](https://github.com/lm3nitro/Projects/assets/55665256/a307952d-c493-49fd-904c-477504a2e6bf)

Keep in mind that It could be necessary to run /sbin/ldconfig or ldconfig as root after the installation.

Check the version: 

nfdump -v



# Convering pcap to nfcapd


![Pasted image 20240402021211](https://github.com/lm3nitro/Projects/assets/55665256/4c2aaef2-ed14-4e4f-b0e1-37a715e88a58)



 nfpcapd -r  ICMP_attack_DoS_attack.pcap  -l .



# Reading nfcapd file with nfdump

![Pasted image 20240402021758](https://github.com/lm3nitro/Projects/assets/55665256/cea98049-d3bf-4355-b064-41bc7ee5f66d)


![Pasted image 20240402022640](https://github.com/lm3nitro/Projects/assets/55665256/5effc527-e197-4202-b838-ff46bdd609f6)


No share:

https://ipcorenetworks.blogspot.com/2021/08/installing-nfsen-nfdump-on-ubuntu-and.html

