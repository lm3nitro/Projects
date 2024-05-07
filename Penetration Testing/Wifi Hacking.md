WIFI HACKING :

![Pasted image 20240506172950](https://github.com/lm3nitro/Projects/assets/55665256/a685d2f4-61f2-4d69-b8ba-9736550094fd)

Updating or installing the dependencies:

![Pasted image 20240506144714](https://github.com/lm3nitro/Projects/assets/55665256/d44ed41e-88ff-4a4b-b222-63ee6f231c13)

![Pasted image 20240506143456](https://github.com/lm3nitro/Projects/assets/55665256/7a259634-c4e4-415a-9e2f-ca446ffc3923)

download drivers:

![Pasted image 20240506143620](https://github.com/lm3nitro/Projects/assets/55665256/b44bbd1e-643b-40c2-9bfc-359b25297ad3)

Compile drivers:

![Pasted image 20240506143809](https://github.com/lm3nitro/Projects/assets/55665256/76c734e3-a443-473f-8e05-a764358a024f)

rebooting:

![Pasted image 20240506144128](https://github.com/lm3nitro/Projects/assets/55665256/53b15088-281f-44df-a1a0-2003ba772fae)

Checking the wireless interface:

![Pasted image 20240506144459](https://github.com/lm3nitro/Projects/assets/55665256/c6c74459-2554-432c-b48a-df809485e34a)


verify wireless interface:


![Pasted image 20240506142957](https://github.com/lm3nitro/Projects/assets/55665256/746c61a6-12d8-4c2e-9ca6-2effe925746f)


Start Airmon-Ng:

![Pasted image 20240506144923](https://github.com/lm3nitro/Projects/assets/55665256/e77a0995-de1c-472d-af9d-eec3bc2f08cd)

![Pasted image 20240506145245](https://github.com/lm3nitro/Projects/assets/55665256/b378b673-3e01-4538-b558-fc46214d9a22)

killing the process 


![Pasted image 20240506170052](https://github.com/lm3nitro/Projects/assets/55665256/f000995c-3999-4ab0-81e2-151a8183e98d)


![Pasted image 20240506151337](https://github.com/lm3nitro/Projects/assets/55665256/a4fe54b5-1e41-4efe-adb1-cea922ded267)



airodump-ng  is  used for packet capturing of raw 802.11 frames for the intent of using them with aircrack-ng. If you have a GPS receiver connected to the computer, airodump-ng is capable of logging the coordinates of the found access points. Additionally, airodump-ng writes  out  a  text  file containing the details of all access points and clients seen


airodump-ng --band abg wlan0mon

![Pasted image 20240506170855](https://github.com/lm3nitro/Projects/assets/55665256/17e32f72-fb26-4c11-b7f3-3ec8c6763f6a)

airodump-ng  wlan0mon  --band abg -d aa:52:xx:32:xx:17  -c 157

![Pasted image 20240506171058](https://github.com/lm3nitro/Projects/assets/55665256/17db4bee-c3b9-4868-8ea8-ef93f140d408)

Capture handshake  traffic:

![Pasted image 20240506171923](https://github.com/lm3nitro/Projects/assets/55665256/920e6385-5802-440f-b8bd-d29226ac36c9)


deauth attack 
![Pasted image 20240506171759](https://github.com/lm3nitro/Projects/assets/55665256/37d1fd5f-aed5-4a3a-ae09-5a6a4366881c)

getting the hanshake:

![Pasted image 20240506203706](https://github.com/lm3nitro/Projects/assets/55665256/b24f592b-21af-4382-aa52-e6985de8ca51)

looking at the pcap capture

![Pasted image 20240506172047](https://github.com/lm3nitro/Projects/assets/55665256/dba870bc-bd1f-445e-8fcf-78566322240e)


cracking the passowrd:

![Pasted image 20240506172828](https://github.com/lm3nitro/Projects/assets/55665256/ccbeb01b-ec58-46ad-891d-23a13d899a7b)


![Pasted image 20240506172412](https://github.com/lm3nitro/Projects/assets/55665256/eaa7f11d-fc65-44f1-8c2c-15468ca6a3f9)


![Pasted image 20240506152027](https://github.com/lm3nitro/Projects/assets/55665256/256f8589-1fdb-4f6e-bc44-dbad54b89594)


Wireless traffic analysis:

Traffic analysis refers to inspecting the captured/stored WiFi traffic to gather information regarding clients, access points and their activities.

![Pasted image 20240506151730](https://github.com/lm3nitro/Projects/assets/55665256/39fb9ff8-519b-484c-8257-dd920724dce0)


Here a few things that you can do with wireless traffic analysis:

Analyzing WiFi traffic to identify WiFi networks and clients

Different WiFi frames and frame structure

Checking client AP connection/disconnection, WPA handshake and SAE handshake

Observing the difference between different types of security schemes

Identifying evil twins and impersonating client from traffic captures




Finding WPS enable devices:

![Pasted image 20240506152325](https://github.com/lm3nitro/Projects/assets/55665256/882bdaf1-22be-427d-885e-0cc8a769f9d5)


Configuring the interface in monitor mode:

![Pasted image 20240506153617](https://github.com/lm3nitro/Projects/assets/55665256/64168ca7-1678-4883-91ca-1eb5c8d667fb)



