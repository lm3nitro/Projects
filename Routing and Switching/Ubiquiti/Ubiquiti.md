# Ubiquiti 

<img width="268" alt="Screenshot 2024-11-11 at 2 31 02 PM" src="https://github.com/user-attachments/assets/a67e6458-00c6-4f54-92be-c90d5138b75c">

Ubiquiti's UniFi Switch is aimed at providing high-performance, scalable networking solutions for both home and business environments. The UniFi Switches are typically used to manage and distribute Ethernet connections within a local area network (LAN), supporting both wired and wireless networking devices.

### SCOPE

I recently purchased the UniFi Switch USW-FLEX-MINI-5 and will walk through the steps I took to set up and configure the switch. The USW-FLEX-MINI-5 is a compact and versatile model, ideal for small networks or tight spaces, and it offers a great balance of features and affordability. I’ll cover the initial setup process and configuration. Additionally, I'll demonstrate how to configure a SPAN port on the switch. This will allow me to mirror the traffic from a specific port, so I can capture and monitor the data being transmitted from the device on that port.

Below is a view at the network diagram:

![Pasted image 20240513174016](https://github.com/lm3nitro/Projects/assets/55665256/d5d2ab6a-5b92-4400-94e5-905cf57225e1)

## Getting Started:

Below is an image of the device itself. It's small and compact:

![Pasted image 20240507173319](https://github.com/lm3nitro/Projects/assets/55665256/6fab0cb6-1565-48a7-9a78-71b91d337165)

I first navigated to the UniFi homepage where I needed to download the installer:

![Pasted image 20240507162952](https://github.com/lm3nitro/Projects/assets/55665256/51729587-4e08-445c-acd1-b14bb771106e)

Once downloaded, I ran the installer:

![Pasted image 20240507163122](https://github.com/lm3nitro/Projects/assets/55665256/b9a4b2dd-0f8d-4887-ba0f-f2358331c7f6)


![Pasted image 20240507163146](https://github.com/lm3nitro/Projects/assets/55665256/619276ec-2dc7-42af-9483-7d7ec7f7f247)

Installer:

![Pasted image 20240507163212](https://github.com/lm3nitro/Projects/assets/55665256/9d085b96-863a-4120-9835-9e346e55861d)

Once complete, the icon appears on the desktop. By selecting it, I was presenrted with the option to manage via the web browser:

![Pasted image 20240507163252](https://github.com/lm3nitro/Projects/assets/55665256/d43e79a1-47f5-4ede-8cf6-c7a0bda21438)

An image of the device in my home and the connected port:

![Pasted image 20240507172450](https://github.com/lm3nitro/Projects/assets/55665256/1c06e3f4-ecc1-46ab-9751-f7e309bdf55f)

Once I selexted to manage via the browser, I was presented with the login page:

![Pasted image 20240507173834](https://github.com/lm3nitro/Projects/assets/55665256/474fc20f-d3d5-40af-a95a-591c535e760e)

Upon logging in I was presented with information on the system:

![Pasted image 20240507163411](https://github.com/lm3nitro/Projects/assets/55665256/fb0b0881-8976-4e0c-b356-08c6a88a5217)

Prior to starting, I first had to adopt the device (the switch itself) in order to manage it:

![Pasted image 20240507162726](https://github.com/lm3nitro/Projects/assets/55665256/92e1212f-dbba-4fa0-8c3f-b76955f4f934)

![Pasted image 20240507163517](https://github.com/lm3nitro/Projects/assets/55665256/1713403c-9729-414d-a2ff-e1ced11c0b3e)

I can see that the switch has been added and can now be managed and I can make changes to it:

![Pasted image 20240507163723](https://github.com/lm3nitro/Projects/assets/55665256/412f41d8-0f05-4930-a435-1280df9cf19d)

Looking at the ports on the switch, I can see that there are currently 2 connections:

![Pasted image 20240507163911](https://github.com/lm3nitro/Projects/assets/55665256/d5e98757-9516-420a-90b3-6b35937c20fe)

Network topology view from the switch:

![Pasted image 20240509155003](https://github.com/lm3nitro/Projects/assets/55665256/05266141-27f8-419e-ab3d-a182193de01b)

Connection status for the connected ports:

![Pasted image 20240507164350](https://github.com/lm3nitro/Projects/assets/55665256/5c4e60b3-97aa-446a-80eb-0a9463114b29)

These is a close-up view of the ports. I will mirroring the traffic from port 1 on ports 5:

![Pasted image 20240507171435](https://github.com/lm3nitro/Projects/assets/55665256/db087b23-03f3-4894-ade8-a5ac7ad37ea0)

To do this, I selected port 5 in order to configure it. Here I am setting ujp port-mirroring:

![Pasted image 20240507171635](https://github.com/lm3nitro/Projects/assets/55665256/f3a4b63a-9912-4549-ba31-ed936732a464)

Physical view of the switch:

![Pasted image 20240507173644](https://github.com/lm3nitro/Projects/assets/55665256/458f0c50-3b27-49b2-84db-f85618fd9e7d)

Next, I verified the port icon had changed. The icon changes depending on the port configuration:

![Pasted image 20240507171722](https://github.com/lm3nitro/Projects/assets/55665256/bd6680d8-f26d-4f08-a87b-e51df13cf258)

## Testing

I then tested. To test, I went to the host that was connected to port 1 on the switch and navigated to `chess.com`:

![Pasted image 20240507172204](https://github.com/lm3nitro/Projects/assets/55665256/969f0f45-2822-4808-a278-0fa5ce97013c)

I had a host connected to the SPAN port that was running Wireshark and captuing all the traffic. From port 5 I was able to see the traffic from the hos ton port 1 going to `chess.com`:

![Pasted image 20240507172117](https://github.com/lm3nitro/Projects/assets/55665256/be38b324-c328-4544-9a0d-fe904668373d)

### Summary:

In this lab, I was able to set up and configure the UniFi Switch USW-FLEX-MINI-5 and implement a SPAN port for traffic mirroring. I was able to gain more experience in the following:

+ Basic Switch Setup and Configuration:
  - How to physically connect and power up a managed switch.
  - Configuring network settings, such as IP addressing, port management, etc.
+ Understanding Port Mirroring:
  - How to configure a SPAN port on a switch to mirror the traffic of specific ports.
  - The purpose of SPAN port configurations for network monitoring, traffic analysis, and troubleshooting.
  - Use of tools such as Wireshark to capture and analyze the mirrored traffic

In summary, this process deepened my understanding of network management, monitoring, and troubleshooting, while providing practical experience with Ubiquiti’s UniFi switch. 

