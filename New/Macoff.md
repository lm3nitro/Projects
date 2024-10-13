

Macof, short for “Mac Flooding,” is a powerful tool used by ethical hackers to perform a MAC address flooding attack on a network. MAC, or Media Access Control, addresses are unique hardware addresses assigned to network devices, such as computers, routers, and switches. Macof works by sending a massive number of ARP (Address Resolution Protocol) requests with random or spoofed MAC addresses to flood a network, thereby overwhelming its switch’s MAC address table.



![Pasted image 20241011131110](https://github.com/user-attachments/assets/a5eb6087-1388-46b2-8406-6e64dad4fe97)


Router IP: 192.168.91.216
Kali Linux : 192.168.128


###  Host TCP/IP information:



![Pasted image 20241011113823](https://github.com/user-attachments/assets/593c0ade-3dd9-411f-b1a0-4ddab051d091)




### Reachability test:


![Pasted image 20241011113842](https://github.com/user-attachments/assets/847cc790-cb0b-4d1d-929f-5491329c96f9)


### Link layer:


![Pasted image 20241011113908](https://github.com/user-attachments/assets/7c80144d-4e90-4f9c-9000-ddf9270387a0)



### Switch information :


```
show interfaces gigabitEthernet 0/1 switchport
```



![Pasted image 20241011111411](https://github.com/user-attachments/assets/3d0185af-8bd8-4b2a-bd6d-69889a7d901b)


```
show interfaces gigabitEthernet 0/1
```


![Pasted image 20241011111458](https://github.com/user-attachments/assets/38acfa81-0ff3-4a23-9643-43511e2b5dc0)


```
show mac address-table interface gigabitEthernet 0/1

```



![Pasted image 20241011111839](https://github.com/user-attachments/assets/70a16751-55cc-4ac1-9fe9-208c9bec7cb8)





# Mac flood attack:



![Pasted image 20241011115206](https://github.com/user-attachments/assets/46b20018-a3a6-4e09-9d5c-f5737f577a11)


### Mac address table:



![Pasted image 20241011114622](https://github.com/user-attachments/assets/c95bc533-50a3-44b7-93e2-840d2a5d6c5e)


###   ARP protocol behaviour: 



![Pasted image 20241011121731](https://github.com/user-attachments/assets/c6148419-96c6-4b99-ab8d-a81da1db5387)


Radom source and destination IP as well Mac address: 


![Pasted image 20241011114906](https://github.com/user-attachments/assets/0fe11941-cd73-44f0-a1af-f77b0d8eb7f2)




# Port security:

Port security is a feature on Cisco switches that allows network administrators to restrict access to the switch ports based on the MAC addresses of connected devices. It helps enhance network security by preventing unauthorized devices from accessing the network. Here are the key aspects of port security:


MAC Address Limitation
Violation Actions
Sticky MAC Addresses
Logging
### configuration:

```
enable
configure terminal
interface GigabitEthernet0/1
 switchport mode accesss
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
end
write memory
```




```
sh interfaces status

```



![Pasted image 20241011120604](https://github.com/user-attachments/assets/7104be41-9532-4472-990f-316467f74eb4)


### Policy violation error message:


![Pasted image 20241011120341](https://github.com/user-attachments/assets/0c097480-9c2c-4047-b05e-9ad02ddce9c7)

```
sh interfaces gigabitEthernet 0/1
```
The interface is down dude to the port security violation: 


![Pasted image 20241011120431](https://github.com/user-attachments/assets/bb453da3-95c2-4514-a29f-f6d7b5b71975)


```
sh mac address-table  interface gi0/1

```


![Pasted image 20241011120705](https://github.com/user-attachments/assets/8b1b2e61-8325-41d4-aaa0-a269f9be5808)
