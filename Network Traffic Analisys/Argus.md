# Argus

Argus is the first network flow system, developed by Carter Bullard in the early 1980's at Georgia Tech, and adapted for cyber security incident response at the first Computer Emergency Response Team (CERT) in Carnegie Mellon's Software Engineering Institute in the late 1980's.

Argus is network audit technology, providing a network activity audit engine for all network traffic.

The Argus architecture is designed to support small and very large scale network auditing.  The real-time data provides a lot of information, which can be stored in files for processing later, or the clients programs can be pieced together to provide real-time network data streams for simple network awareness, large scale distributed visibility, even active cyber defense.

Argus packages for Linux distros are maintained by a diverse group of teams and individuals, as a result Argus is available for RedHat, Debian, Suse, and Ubuntu using the native software management tools.  For Ubuntu, just as an example:


 sudo apt install argus-server  
 sudo apt install argus-clients


![Pasted image 20240402150020](https://github.com/lm3nitro/Projects/assets/55665256/77382312-bc0f-4206-a883-967d9db44de0)


# Getting help:

argus -h 

![Pasted image 20240402150554](https://github.com/lm3nitro/Projects/assets/55665256/c38447ee-365f-42dc-866b-cc011e03d9e5)


# Flow analysis with Argus:

![Pasted image 20240402155216](https://github.com/lm3nitro/Projects/assets/55665256/160f952f-5798-42e5-892e-8ec5064f336b)


![Pasted image 20240402160616](https://github.com/lm3nitro/Projects/assets/55665256/53649c0f-2b1e-4080-8505-0730f9205f96)



# More examples:

 
  Run argus as a daemon, writing all its transaction status reports to output-file.  This is the typical mode.
 argus -d -e `hostname` -w output-file


If ICMP traffic is not of interest to you, you can filter out ICMP packets on input.
  argus -w output-file - ip and not icmp

 
 Audit each individual ICMP ECHO transaction from data in <dir>.  You would do this to gather Round Trip Time (RTT) data within your network.  Append the output to output-file.

argus -R dir -w output-file "echo" - icmp


 Import flow data from pcap file containing Cisco flow data packets. Write output to stdout, to a ra.1 instance.
 argus -r paloalto:pcap-file -w - | ra 
