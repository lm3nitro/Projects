
# Bettercap

![Pasted image 20240517142357](https://github.com/lm3nitro/Projects/assets/55665256/8a88a4bd-94aa-4b31-8bc5-a702b1bef769)

Bettercap is a powerful, flixible, and easy-to use network security tool designed for security assesments, including network monitoring, analysis, and attacks. It is particularly popular and used for tasks related to man-in-the-middle (MITM) attacks, packet sniffing, and protocol manipulation. 

### Scope:


### Tools and Technology:

### Network Diagram:

![Pasted image 20240517211750](https://github.com/lm3nitro/Projects/assets/55665256/51f63d6e-5599-48bf-a67a-5b8bc239fbbb)


Searching for bettercap:

![Pasted image 20240517142617](https://github.com/lm3nitro/Projects/assets/55665256/0ddcc118-2dfe-4e2e-99b5-b64bef0c2077)

## Installing bettercap


![Pasted image 20240517142653](https://github.com/lm3nitro/Projects/assets/55665256/44207384-19c5-49cd-b75a-928f3b3c5b9d)


Getting help with bettercap:

![Pasted image 20240517142727](https://github.com/lm3nitro/Projects/assets/55665256/6361046b-87f3-4c3f-8132-d9fa5646251d)




Starting bettercap:

bettercap 

![Pasted image 20240517150334](https://github.com/lm3nitro/Projects/assets/55665256/441179a6-7878-4923-b27b-db0454ec02b7)





Probing the network:

net.probe on

![Pasted image 20240517150556](https://github.com/lm3nitro/Projects/assets/55665256/0ef68117-6424-4920-b730-a57339329116)


Listing the gateway and the hosts on the network:



net.show


![Pasted image 20240517150641](https://github.com/lm3nitro/Projects/assets/55665256/d99211bd-bc19-417d-8778-45e448642d8a)



Enabling ARP spoofing :

![Pasted image 20240517151238](https://github.com/lm3nitro/Projects/assets/55665256/8335f958-52e2-4602-9e42-2678e36b51c2)




At the windows PC "Lm3nitro-PC":


![Pasted image 20240517153137](https://github.com/lm3nitro/Projects/assets/55665256/cfc0e03a-d977-41d0-be4c-0e350673104e)


![Pasted image 20240517153359](https://github.com/lm3nitro/Projects/assets/55665256/4566ad42-6498-4074-8fea-8c32f838b886)


We can see a duplicate IP:


![Pasted image 20240517151220](https://github.com/lm3nitro/Projects/assets/55665256/d0886a11-098e-4890-b706-a01a593becfd)





Enabling sniffing:

net.sniff on

We can see lm3nitro-PC visiting chess.com and the traffic has been sniffed by the attacker pc on the bottom 

![Pasted image 20240517155121](https://github.com/lm3nitro/Projects/assets/55665256/6d844ee1-6b37-4130-859f-e4e5854af2ab)


lm3nitro-PC :

![Pasted image 20240517154646](https://github.com/lm3nitro/Projects/assets/55665256/893c4f6e-7293-4c0e-b8ef-faec5eefd9d4)





Stteing UP DNS spfoing attack:


Starting a webserver:


![Pasted image 20240517155521](https://github.com/lm3nitro/Projects/assets/55665256/f28423bf-ce28-4477-98c6-8d2c51c419c0)


setting up dns spoof:


set. dns.spoof.domains chess.com
dns.spoof on


![Pasted image 20240517155654](https://github.com/lm3nitro/Projects/assets/55665256/3a0b38a0-9814-4660-88ae-192c2cdbb5cb)


On the lm3nitro-PC:


Let's go to chess.com again:

![Pasted image 20240517160544](https://github.com/lm3nitro/Projects/assets/55665256/c27d07f4-4f1f-44e1-858b-1d853c5bf823)


Output from the Attacker PC:

![Pasted image 20240517160818](https://github.com/lm3nitro/Projects/assets/55665256/b0c70da2-b6ef-4a3f-8c12-59a9e753f62e)









Detecting ARP spoof:



Installing arpwatch on PFSEBSE:





![Pasted image 20240517161852](https://github.com/lm3nitro/Projects/assets/55665256/abf38603-7b63-4844-91a7-7adaed9b0e7c)


![Pasted image 20240517162104](https://github.com/lm3nitro/Projects/assets/55665256/4a1e2daa-b0e9-4c8e-ae04-848afcc1b1c1)



![Pasted image 20240517162135](https://github.com/lm3nitro/Projects/assets/55665256/a697389b-ca83-4ffc-b9ec-2235ecdc3b4d)






configurating arpwatch :


![Pasted image 20240517162226](https://github.com/lm3nitro/Projects/assets/55665256/f04eae82-633f-40f3-a61c-6cf91a208879)





![Pasted image 20240517162409](https://github.com/lm3nitro/Projects/assets/55665256/71791e98-3c8b-495f-9079-ba2bf3198aaf)



Normal ARP table:


![Pasted image 20240517162815](https://github.com/lm3nitro/Projects/assets/55665256/447d185a-000a-4d04-9fe9-c7e223feeca7)





Spoof ARP table (duplicate 192.168.1.1 IP)

![Pasted image 20240517162843](https://github.com/lm3nitro/Projects/assets/55665256/6898b8c6-bd43-4d3a-a024-6e500ed74f50)



On the network :

![Pasted image 20240517164505](https://github.com/lm3nitro/Projects/assets/55665256/7c0baf7f-f1f6-41b0-af68-92fe0f7e33d8)

 A lot of ARP traffic or broadcast traffic from a single IP:
 
![Pasted image 20240517164714](https://github.com/lm3nitro/Projects/assets/55665256/4039cdf0-8d98-47f2-a57d-0b3d949ffcff)



