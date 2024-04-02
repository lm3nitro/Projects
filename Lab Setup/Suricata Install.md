## Install Suricata

Suricate is an open source IDS/IPS. Suricata currently supports a wide range of network protocols and file types. This provides a comprehensive visibility into network traffic for effective threat hunting and analysis. I also like that the rules can be fine tuned to fit your environment and needs.

Here I installed the latest stable version of Suricata from Personal Package Archives (PPA)
```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata
```

>##### Upgrade Suricata:
```
sudo apt-get update
sudo apt-get upgrade suricata
```
Once I installed and upgraded suricata, I then began to send logs to Splunk.

Flow:

![Pasted image 20240331164810](https://github.com/lm3nitro/Projects/assets/55665256/0d0d8eba-1f96-4317-9f92-fd9af9c9248b)

Alerts:

![Pasted image 20240331164627](https://github.com/lm3nitro/Projects/assets/55665256/62b89c67-a98e-43e0-8c26-eed13331cfd9)

Flow in detailed json format: 

![Pasted image 20240331164454](https://github.com/lm3nitro/Projects/assets/55665256/c5f548c7-b219-4d84-b1a7-b0c95382bc78)


