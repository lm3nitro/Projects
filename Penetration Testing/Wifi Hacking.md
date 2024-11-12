# WiFi Hacking

![Pasted image 20240506172950](https://github.com/lm3nitro/Projects/assets/55665256/a685d2f4-61f2-4d69-b8ba-9736550094fd)

### Scenario:

I set up a personal home network but forgot the Wi-Fi password after changing it a few months ago. To get the password, I will be conducting a controlled experiment to crack my own Wi-Fi password using ethical hacking techniques. This scenario serves as an important learning experience, demonstrating the ethical implications of hacking practices and the necessity of maintaining robust security measures.

### Tools and Technologies:
Linux VM, Airodump, Aircrack-ng, and Wireshark

## Getting Started

To get started, I first updated current packages and installed the dependencies needed on my VM:

```
apt install linux-headers-generic build-essential git
```

![Pasted image 20240506144714](https://github.com/lm3nitro/Projects/assets/55665256/d44ed41e-88ff-4a4b-b222-63ee6f231c13)

## Aircrack-ng

<img width="222" alt="Screenshot 2024-10-05 at 4 13 31 PM" src="https://github.com/user-attachments/assets/187edc8c-9411-4804-89ac-982d37d3d28a">

Next, I needed to get aircrack-ng. Aircrack-ng is a suite of tools designed for assessing the security of wireless networks. It primarily focuses on different aspects of Wi-Fi security, including monitoring, attacking, testing, and cracking WEP and WPA/WPA2 encryption keys. 

![Pasted image 20240506143456](https://github.com/lm3nitro/Projects/assets/55665256/7a259634-c4e4-415a-9e2f-ca446ffc3923)

Downloaded the drivers:

![Pasted image 20240506143620](https://github.com/lm3nitro/Projects/assets/55665256/b44bbd1e-643b-40c2-9bfc-359b25297ad3)

Compiled and installed the drivers:

```
make && make install
```
![Pasted image 20240506143809](https://github.com/lm3nitro/Projects/assets/55665256/76c734e3-a443-473f-8e05-a764358a024f)

Rebooted:

```
shutdown -r 0
```

![Pasted image 20240506144128](https://github.com/lm3nitro/Projects/assets/55665256/53b15088-281f-44df-a1a0-2003ba772fae)

Once rebooted, I checked the wireless interface:

```
ifconfig
```

![Pasted image 20240506144459](https://github.com/lm3nitro/Projects/assets/55665256/c6c74459-2554-432c-b48a-df809485e34a)

Verified the wireless interface:

```
lsusb
```

![Pasted image 20240506142957](https://github.com/lm3nitro/Projects/assets/55665256/746c61a6-12d8-4c2e-9ca6-2effe925746f)

## Airmon-Ng:

Airmon-ng is a tool that is part of the Aircrack-ng suite, specifically designed for managing wireless interfaces in monitor mode.

```
airmon-ng start wlan0
```

![Pasted image 20240506144923](https://github.com/lm3nitro/Projects/assets/55665256/e77a0995-de1c-472d-af9d-eec3bc2f08cd)

Configuring the interface in monitor mode:

![Pasted image 20240506153617](https://github.com/lm3nitro/Projects/assets/55665256/64168ca7-1678-4883-91ca-1eb5c8d667fb)

```
ifconfig
```

![Pasted image 20240506145245](https://github.com/lm3nitro/Projects/assets/55665256/b378b673-3e01-4538-b558-fc46214d9a22)

I then killed the process 

```
airmon-ng check kill
```

![Pasted image 20240506170052](https://github.com/lm3nitro/Projects/assets/55665256/f000995c-3999-4ab0-81e2-151a8183e98d)

Optional: Finding WPS enabled devices:

![Pasted image 20240506152325](https://github.com/lm3nitro/Projects/assets/55665256/882bdaf1-22be-427d-885e-0cc8a769f9d5)

## Airodump-ng

![Pasted image 20240506151337](https://github.com/lm3nitro/Projects/assets/55665256/a4fe54b5-1e41-4efe-adb1-cea922ded267)

Airodump-ng is used for packet capturing of raw 802.11 frames for the intent of using them with aircrack-ng. If you have a GPS receiver connected to the computer, airodump-ng is capable of logging the coordinates of the found access points. Additionally, airodump-ng writes  out  a  text  file containing the details of all access points and clients seen.

```
airodump-ng --band abg wlan0mon
```

![Pasted image 20240506170855](https://github.com/lm3nitro/Projects/assets/55665256/17e32f72-fb26-4c11-b7f3-3ec8c6763f6a)

```
airodump-ng  wlan0mon  --band abg -d aa:52:xx:32:xx:17  -c 157
```

![Pasted image 20240506171058](https://github.com/lm3nitro/Projects/assets/55665256/17db4bee-c3b9-4868-8ea8-ef93f140d408)

I was also able to capture the handshake traffic:

![Pasted image 20240506171923](https://github.com/lm3nitro/Projects/assets/55665256/920e6385-5802-440f-b8bd-d29226ac36c9)

## Deauthentication Attack

This attack exploits the IEEE 802.11 Wi-Fi standard, specifically the management frame called the deauthentication frame, which is used by the network to notify devices about their disconnection from the network. 

![Pasted image 20240506171759](https://github.com/lm3nitro/Projects/assets/55665256/37d1fd5f-aed5-4a3a-ae09-5a6a4366881c)

Getting the handshake:

![Pasted image 20240506203706](https://github.com/lm3nitro/Projects/assets/55665256/b24f592b-21af-4382-aa52-e6985de8ca51)

Looking at the pcap capture:

![Pasted image 20240506172047](https://github.com/lm3nitro/Projects/assets/55665256/dba870bc-bd1f-445e-8fcf-78566322240e)

## Cracking the passowrd:

> [!TIP]
> Use a good wordlist: The success of this method depends heavily on the quality of your password list. The more comprehensive it is, the higher your chances of success.
> Optimize your wordlist: If you know the potential patterns of the password (e.g., common words, numbers), you can create a custom wordlist tailored to those expectations.
> Time-consuming: Cracking can take a considerable amount of time, especially with longer and more complex passwords.

I used aircrack-ng along with the pcap and the rockyou wordlist:

```
aircrack-ng handshake.pcap-01.cap -w rockyou.txt
```

![Pasted image 20240506172412](https://github.com/lm3nitro/Projects/assets/55665256/eaa7f11d-fc65-44f1-8c2c-15468ca6a3f9)

As seen in the screenshot, I was successful in cracking and obtaining the password: `hackme-lm3nitro`

## Wireless Traffic Analysis:

![Pasted image 20240506152027](https://github.com/lm3nitro/Projects/assets/55665256/256f8589-1fdb-4f6e-bc44-dbad54b89594)

Traffic analysis refers to inspecting the captured/stored WiFi traffic to gather information regarding clients, access points and their activities.

![Pasted image 20240506151730](https://github.com/lm3nitro/Projects/assets/55665256/39fb9ff8-519b-484c-8257-dd920724dce0)

Here a few things that you can do with wireless traffic analysis:

 + Analyzing Wi-Fi traffic to identify Wi-Fi networks and clients
 + Different Wi-Fi frames and frame structure
 + Checking client AP connection/disconnection, WPA handshake and SAE handshake
 + Observing the difference between different types of security schemes
 + Identify evil twins and impersonating client from traffic captures

### Summary

By going through the process of capturing the handshake and attempting to crack the password, I was able to gain a deeper understanding of how Wi-Fi security protocols (like WEP, WPA, and WPA2) function, as well as their vulnerabilities. Attempting to crack my own Wi-Fi password also helped me to understand the importance of using strong, complex passwords. 
