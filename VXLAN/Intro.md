# VXLAN-EVPN

<img width="507" alt="Screenshot 2024-11-16 at 8 29 09 PM" src="https://github.com/user-attachments/assets/8725fc28-76e8-4fc0-bef0-8145e92c8159">

For this exercise of configuring VXLAN, I will be using EVE-NG. EVE-NG is a network emulation tool that allows users to design, configure, and test network topologies in a virtual environment. I have installed EVE-NG bare metal on my server in my home. VXLAN-EVPN  is a technology used in modern data center networking to address challenges related to scalability, multi-tenancy, and mobility. VXLAN works with BGP primarily through the use of BGP EVPN  to manage the control plane for VXLAN overlay networks. Below is a topology of what i will be configuring:

![Pasted image 20240426115032](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/df580b3b-3c7c-48c6-9435-d56c5748b687)

### VXLAN Benefits

+ VXLAN supports up to 16 million logical networks (compared to VLAN's 4096), making it ideal for large-scale deployments.
+ It enables more granular segmentation of networks, allowing for better isolation and security of workloads.
+ It supports multitenancy by allowing multiple tenants to coexist on the same physical infrastructure without interfering with each other.
+ VXLAN works well in cloud, virtualized, and hybrid environments, making it versatile for modern infrastructure.

### Use Cases

+ VXLAN can connect multiple data centers over long distances, allowing for seamless workload migration and replication.
+ VXLAN supports the dynamic movement of virtual machines (VMs) across different hosts and data centers without reconfiguring the underlying network.
+ If you need a large number of virtual networks, VXLAN offers a scalable solution that exceeds traditional VLAN limits.

### Deployment Information:

VXLAN-EVPN requires a robust IP underlay network for transport. This underlay network provides connectivity between VXLAN tunnel endpoints (VTEPs) across the data center fabric. When it comes to VXLAN, there are 2 main ways to configure it, VLAN-Aware and VLAN-Based. Below are some of the main differences between the two:

1. VLAN-Based
 + In this approach, each VLAN is mapped to a VXLAN Network Identifier (VNI), allowing for the encapsulation of Ethernet frames from those VLANs into VXLAN packets. This method is beneficial if you wish to preserve their existing VLAN infrastructure while gaining the scalability and flexibility of VXLAN.

2. VLAN-Aware
 + In this approach, VXLAN extends the capabilities of traditional VLANs by allowing multiple VLANs to be represented within a single VXLAN segment. This means that a single VXLAN can encapsulate traffic from multiple VLANs, creating a more efficient network architecture. This approach is useful in multitenant environments, as it enables better resource utilization and simplifies network management by reducing the number of required overlays.

## Getting Started:

Below are some details of the EVE-NG server where it is installed. I have it installed on Ubuntu 20.04:

![Pasted image 20240426123059](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/87ddd73a-87c2-4de5-bf42-fec2a30dec26)

Information about the system, CPU, threads, etc:

![Pasted image 20240426123317](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/9c113003-a94e-4e7b-8c32-f7bf68546fce)

Information about the interfaces on the server:

![Pasted image 20240426123612](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/6dd4fb8a-b9f8-49f1-8afc-8345bff1121b)

Below is a screenshot on NAT that I have configured in order for EVE-NG to work properly:

![Pasted image 20240426123708](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/721cbc2d-dd4c-4c09-b0f3-1c834c493e11)

I have several images installed including Cisco, Palo Alto, etc.:

![Pasted image 20240426124053](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/778e19d1-71c4-492e-86de-c031d9781544)

![Pasted image 20240426124114](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/7f1f5295-c42f-4e9e-b3bc-48a3fceaeb03)

Once EVE-ng is installed and running, below if the main login page:

![Pasted image 20240426121612](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/097f63c2-0caa-4331-a287-471bf44c069d)

Once logged in, you can see more details about the system itself:

![Pasted image 20240426122022](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/a1a34fb8-e6ea-4ffa-a9a1-d7a6891a029d)

Including CPU: 

![Pasted image 20240426122221](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/2fee1176-9171-42f3-b545-b1b7325f7e2f)
You can also see full system details:

![Pasted image 20240426121437](https://github.com/lm3nitro/VXLAN-EVPN/assets/55665256/63f0ffb2-201e-41c1-9cdc-a3011ca4a096)

### Summary

In this lab I was able to configure both VLAN-Aware and VLAN-Based approaches. Each configuration will be included below: 

+ [VLAN-Aware](https://github.com/lm3nitro/CyberLabs/tree/main/VXLAN/VLAN-Aware)
+ [VLAN-Based](https://github.com/lm3nitro/CyberLabs/tree/main/VXLAN/VLAN-Based)

