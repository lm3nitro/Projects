


![Pasted image 20240430150728](https://github.com/lm3nitro/Projects/assets/55665256/4c844034-1d00-45f5-aece-fc84dd32fecc)


Ports 135, 139, 445 are open Let's run all the SMB scripts to see if we can find something then later we can check 3389

![Pasted image 20240430130929](https://github.com/lm3nitro/Projects/assets/55665256/7a02251b-f8e6-4ddb-8755-b90fca42c2fb)


Lets zoom in to see the vulnerability better:


![Pasted image 20240430131124](https://github.com/lm3nitro/Projects/assets/55665256/2394fd85-53da-4d6c-a2f1-3fc45a1272fd)



Let's look for all the NSE scripts related to SMB:

![Pasted image 20240430131406](https://github.com/lm3nitro/Projects/assets/55665256/dcd0c45d-f649-4dc7-9529-23ff29195fa4)



Example of using specify NSE script:

![Pasted image 20240430131631](https://github.com/lm3nitro/Projects/assets/55665256/e45b47fa-8ac3-40f4-857c-a636e8a8c1e0)


Using Nessus :

![Pasted image 20240430125818](https://github.com/lm3nitro/Projects/assets/55665256/d8eb4157-fced-4087-a586-b6e431edcc02)

![Pasted image 20240430125946](https://github.com/lm3nitro/Projects/assets/55665256/4d24cee6-82ae-4b9f-98d0-1ce40afaf672)



# Running Metasploit:

![Pasted image 20240430132250](https://github.com/lm3nitro/Projects/assets/55665256/85d94b75-c48b-4b44-86a4-4ffb72892a54)


Searching for a Xploit:

![Pasted image 20240430132540](https://github.com/lm3nitro/Projects/assets/55665256/9f23abb8-0ccb-4f2c-8f7d-13f31408e29b)


Using a auxiliary scanner:


![Pasted image 20240430133026](https://github.com/lm3nitro/Projects/assets/55665256/9156e165-f04d-465a-ab12-29ded9b478e6)

![Pasted image 20240430133353](https://github.com/lm3nitro/Projects/assets/55665256/a3893b46-0d83-4071-ac52-b7643cd28f40)


set RHOST 192.168.91.137
set LHOST 192.168.91.134


![Pasted image 20240430140159](https://github.com/lm3nitro/Projects/assets/55665256/1f0c4455-65d3-4bc3-9aa3-1e08199137c3)


Looking at the exploit on the Network traffic:

![Pasted image 20240430145719](https://github.com/lm3nitro/Projects/assets/55665256/7fea045a-b95f-4977-81d7-9109ec58315a)

![Pasted image 20240430145921](https://github.com/lm3nitro/Projects/assets/55665256/c981eb3c-ef69-474d-a44b-4eda2925c46b)


Upgrading Meterpreter shell (if needed/optional)


![Pasted image 20240430141151](https://github.com/lm3nitro/Projects/assets/55665256/7da1055c-2225-4b7e-a998-a2129f24fdd6)



Getting into the back to the Session:

![Pasted image 20240430141305](https://github.com/lm3nitro/Projects/assets/55665256/0660bea4-2b41-42e8-917c-9c40fbbd00a7)

![Pasted image 20240430141716](https://github.com/lm3nitro/Projects/assets/55665256/f527d86a-2b9a-4dae-9334-cc0327e423d3)


Let's go to the user directory:

![Pasted image 20240430142147](https://github.com/lm3nitro/Projects/assets/55665256/6fe119f1-e1fb-4311-91ca-acb673fbffa2)


Creating a directory :

![Pasted image 20240430142553](https://github.com/lm3nitro/Projects/assets/55665256/86326d78-c2cb-4386-be07-6af2699bf3a3)


On the windows 7 box , we have created a directory called lm3nitro:

![Pasted image 20240430142453](https://github.com/lm3nitro/Projects/assets/55665256/b3a48d82-9cee-40f1-b65e-eaf8b5024d02)


We can the se reverse meterperter Shell: 

#  Dumping the hash and Cracking it :


Let's crack user m3nitro password:


![Pasted image 20240430145228](https://github.com/lm3nitro/Projects/assets/55665256/2649f103-281e-49e6-a44a-7be91ddba771)


Creating a file to crack it with john the repaper

![Pasted image 20240430145322](https://github.com/lm3nitro/Projects/assets/55665256/dfab06df-d70f-4b52-b697-7cd559552dd7)



using john to crack the hash:

![Pasted image 20240502094456](https://github.com/lm3nitro/Projects/assets/55665256/50b529a5-68e1-4a2f-8487-dfdacd256f62)


![Pasted image 20240430145055](https://github.com/lm3nitro/Projects/assets/55665256/9e1d6c18-17b8-47aa-bdcb-55de62f518d9)


Hascat:

Selecting  NTLM mode:

![Pasted image 20240506102724](https://github.com/lm3nitro/Projects/assets/55665256/1027efb9-96a0-44a8-8483-719bb9c9319f)


Cracking the password:

![Pasted image 20240506104207](https://github.com/lm3nitro/Projects/assets/55665256/3b8659c0-2377-45a8-a40f-00cfa3b7c248)


Mimikatz:

![Pasted image 20240506105845](https://github.com/lm3nitro/Projects/assets/55665256/5d1c8c7a-5e20-4e13-b8e0-c4a6debed536)

![Pasted image 20240506111530](https://github.com/lm3nitro/Projects/assets/55665256/8f6cf77c-991d-4594-9efb-ea7d2242d4ce)


All the creds:

![Pasted image 20240506111623](https://github.com/lm3nitro/Projects/assets/55665256/b959c579-700e-482a-beb8-5ae3f6ff927c)

Remote desktop:

![Pasted image 20240430151932](https://github.com/lm3nitro/Projects/assets/55665256/199e2404-476d-4db3-b72d-1063a559af6a)


logged in:


![Pasted image 20240430151350](https://github.com/lm3nitro/Projects/assets/55665256/3739ff7a-8c28-4ddc-9661-8e45c71d2d87)


LIsting the established connection:

![Pasted image 20240430152107](https://github.com/lm3nitro/Projects/assets/55665256/b098f300-75ec-42e5-84e2-98f6717b1049)





