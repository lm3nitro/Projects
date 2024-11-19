# mitmproxy

![Pasted image 20240605001736](https://github.com/lm3nitro/Projects/assets/55665256/c8be1c54-5f9c-44ea-bc83-74d7b5fcde2a)

mitmproxy is an open-source tool that serves as a Man-in-the-Middle (MITM) proxy specifically designed for intercepting, inspecting, modifying, and replaying web traffic. This is a swiss-army knife for debugging, testing, privacy measurements, and penetration testing. It can be used to intercept, inspect, modify and replay web traffic such as HTTP/1, HTTP/2, WebSockets, or any other SSL/TLS-protected protocols. Below are some of its key features:

+ Traffic Interception
+ Decryption
+ Web Interface
+ Support for WebSockets and HTTP/2
+ Replay and Testing

### Scope:

In this lab, I will be installing and configuring mitmproxy to serve as an effective man-in-the-middle proxy for intercepting and analyzing network traffic. After setting up mitmproxy, I will configure a Windows 10 VM to route its web traffic through the proxy, ensuring that the necessary proxy settings are applied to facilitate this connection. To analyze the decrypted traffic, I will utilize Wireshark in conjunction with the SSLKEYLOGFILE which allows for the logging of SSL session keys used during the HTTPS connections. 

### Tools and Technology:
mitmproxy, Win10, PowerShell and Wireshark

## Downloading mitmproxy

In order to use mitmproxy, I needed to download it. I will be using it on my Win10 VM, so that is the installer I chose:

![Pasted image 20240604220241](https://github.com/lm3nitro/Projects/assets/55665256/177b1031-420c-4cb7-be0d-6952a42d623f)

Once downloaded, I initialized the installation and needed to go through the setup. 

![Pasted image 20240604220323](https://github.com/lm3nitro/Projects/assets/55665256/beac9a3d-eb9c-4978-a85c-af56d202f770)


![Pasted image 20240604220355](https://github.com/lm3nitro/Projects/assets/55665256/655d588f-2e77-42a6-96df-d81e2d10680b)


![Pasted image 20240604220425](https://github.com/lm3nitro/Projects/assets/55665256/5d07e47a-05d2-4e94-b056-829465e71810)


After installating mitmproxy, mitmdump and mitmweb are also added to the path and can be invoked from the command line.
 
+ mitmproxy: Gives you an interactive command-line interface
+ mitmweb: Gives you a browser-based GUI
+ mitmdump: Gives you non-interactive terminal output

After the installation, I opened windows store and downloaded Windows Terminal.

![Pasted image 20240604221456](https://github.com/lm3nitro/Projects/assets/55665256/5af3330a-ffff-4acc-94c3-9a81ef573f6f)

It should look like this:

![Pasted image 20240604221625](https://github.com/lm3nitro/Projects/assets/55665256/15bce69f-41ae-4828-a725-47118d7ef55e)

## mitmproxy Configuraiton

The first thing I did was verify that the port is listening and cross verified with the the information seen in Task Manager:

![Pasted image 20240604222301](https://github.com/lm3nitro/Projects/assets/55665256/e4d7b705-1c76-4978-b89d-ef0ab59f8339)

Next, I need to configure the proxy settings on my Win10 host. I navigated to the Start Menu and went to Proxy settings. Here I filled out the information and port details for the proxy:

![Pasted image 20240604223043](https://github.com/lm3nitro/Projects/assets/55665256/d2d61c24-4d0d-4337-b6a4-77f91aadffad)

## Proxy Certificate

Once the proxy information was configured, I browsed to `http://mitm.it/` in order to obtain the mitmproxy certificate:

![Pasted image 20240604223415](https://github.com/lm3nitro/Projects/assets/55665256/36d24988-8056-4a0d-8a5b-4815546816ff)

After downloading the cert, I imported the certificate to the root store:

![Pasted image 20240604223548](https://github.com/lm3nitro/Projects/assets/55665256/2d7a2598-f5e5-43d0-82e6-6f8c391a0ed1)

![Pasted image 20240604223621](https://github.com/lm3nitro/Projects/assets/55665256/452f2729-1e14-41b0-8ec5-59828bc34e94)

![Pasted image 20240604223710](https://github.com/lm3nitro/Projects/assets/55665256/31847580-956b-469b-8dd7-9f39deca492f)

![Pasted image 20240604223822](https://github.com/lm3nitro/Projects/assets/55665256/c7c0b38c-1ae9-4766-864a-4ebe86c95cc6)

![Pasted image 20240604223848](https://github.com/lm3nitro/Projects/assets/55665256/d4aab844-cfcc-4ab1-9cdc-2010aade23dd)

## Traffic analysis with mitmproxy:

The mitmproxy configuration is complete, it was time to test. I started by navigating to `chess.com`. I was able to see all the details 

![Pasted image 20240604224229](https://github.com/lm3nitro/Projects/assets/55665256/8d45b525-f19f-4be9-a5a2-0cfd9baa790c)

Sniffing traffic on the loopback interface. I can see that traffic is passing through the proxy:

![Pasted image 20240604232338](https://github.com/lm3nitro/Projects/assets/55665256/059505c3-61b9-41ce-89ae-002373675e06)

Next I went to `lichess.org`:

![Pasted image 20240604224419](https://github.com/lm3nitro/Projects/assets/55665256/9424ea5c-9b45-45a6-a191-d37a920e82ba)

By clicking on a particular flow, I was presented with more information:

![Pasted image 20240604225332](https://github.com/lm3nitro/Projects/assets/55665256/0a752085-63f7-4aaa-9126-9764462b6f41)

As an example, on the lower left-hand side I can get information about the type of web browser and the operating system. On the upper left-hand side, I can see the Reponses from the proxy server (in this case lichess.org is using an Ngnix proxy). The other two tabs on the right-hand side are information about the domain certificates and the socket.

## Setting Up the SSLKEYLOGFILE for Decryption:

Next, in a terminal I ran the following inside of the .mitmproxy directory which is installed by default inside the home directory. This command sets the SSLKEYLOGFILE variable which I will use to write the session keys to.

```
$env:SSLKEYLOGFILE="~/.mitmproxy/sslkeylogfile.txt"
```

![Pasted image 20240604233637](https://github.com/lm3nitro/Projects/assets/55665256/e54841fa-5216-460d-a345-8cbf4f1f88dd)

After, I started the mitmproxy on another terminal by typing: `Use mitmproxy.exe`

I then verified if the session keys were being saved on the file:

![Pasted image 20240604233828](https://github.com/lm3nitro/Projects/assets/55665256/0bb64c25-b606-4b5d-a7c0-210354d9d0d9)

The file contains all the sessions keys generated by the browser:

![Pasted image 20240605151545](https://github.com/lm3nitro/Projects/assets/55665256/6494ee84-7d86-45e9-ae34-9609fe83542f)

> [!NOTE]  
> Another way to create the env SSLKEYLOGFILE:
> Go to the start menu > Edit the system environment variable.
> ![Pasted image 20240605152218](https://github.com/lm3nitro/Projects/assets/55665256/531758d7-bb11-4c61-9a41-de0c3bf74d44)
> Click on “Advanced System Settings”
> Go to the “Advanced” tab, and click “Environment Variables”
> ![Pasted image 20240605152644](https://github.com/lm3nitro/Projects/assets/55665256/21d652d0-ebb6-4419-a962-569b6a5151c8)
> Click on “New” in the User Variables section. A new dialog box will open.  Give it the following Variable Name: SSLKEYLOGFILE then enter the name of the file and make sure to select 'Browse Directory'. Choose the path you would like; any directory will work. Once the directory has been chosen, add the name of the file to the directory path at the end. The file must end in .txt or .log.
> This will make it so that the file is created since the file does not yet exist in the location. 
Note: Choosing 'Browse File' will not work because the file doesn't yet exist. Choosing this option will not create the file. 

## Configuring Wireshark to use SSLKEYLOGFILE:

Configuring Wireshark to use an SSLKEYLOGFILE allows you to decrypt TLS (SSL) traffic captured during a session. To do this, I did the following:

1. Opened Wireshark
2. Went to Edit > Preferences
3. In the Preferences menu selected Protocols and scrolled down to TLS

![Pasted image 20240604234641](https://github.com/lm3nitro/Projects/assets/55665256/9b715777-0586-4a39-b4bc-fcaf5bed1941)

4. Clicked browse and chose the file:

![Pasted image 20240604234138](https://github.com/lm3nitro/Projects/assets/55665256/0c0ef699-a805-4283-828e-2517f18b2d2c)

5. Selected TLS and pointed the Pre-Master-Secret log filename to the sslkeylogfile.txt

## Start capturing traffic:

Now that I had that set, I started to capture traffic:

![Pasted image 20240605001059](https://github.com/lm3nitro/Projects/assets/55665256/d80f0f2d-beb6-4c1c-a903-b5cb5472ebcf)

While Wireshark was running, I searched within the mitmproxy and filtered for `Linux.org`

![Pasted image 20240605000051](https://github.com/lm3nitro/Projects/assets/55665256/c3be4c95-2522-441b-9011-dc3146f89e2d)

In Wireshark I could also see the Decrypted Traffic:
![Pasted image 20240605000850](https://github.com/lm3nitro/Projects/assets/55665256/ace1abb9-d51e-49a8-9de4-9cdc4957d0cb)

In the bottom right corner, I was also able to see a pane named Decrypted TLS which indicates that Wireshark has successfully decrypted this traffic with the SSLKEYLOGFILE. 

### Summary:

I was able to install and configure mitmproxy to analyze web traffic. I gained hands-on experience in setting up proxy tools, configuring network settings, and understanding how traffic flows through a system. By intercepting and inspecting the traffic, I learned how queries are structured, saw the difference between plaintext and encrypted traffic, and identified potential security risks. This process also deepened my knowledge of SSL/TLS certificates, troubleshooting network issues, and spotting anomalies in traffic that could indicate an attack. 

This also provided hands-on experience with network traffic interception and manipulation, which allowed me to better understand how communication protocols like HTTPS/DNS work and can be exploited. This process helped me see firsthand how attackers might intercept or manipulate traffic, enhancing my ability to secure the network.

Possible use cases:

+ By intercepting and decrypting HTTPS traffic it allows you to observe how malware can communicate with command and control (C2) servers. This can help identify the types of data being exfiltrated or commands being received.
+ It allows you to capture and modify payloads sent or received by malware. This can provide insights into the structure and functionality of the malware.
+ By decrypting traffic, you can analyze response times and payload sizes, identifying performance bottlenecks in the communication between clients and servers.
