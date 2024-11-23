# VXLAN-EVPN

<img width="1009" alt="Screenshot 2024-11-22 at 11 10 41 PM" src="https://github.com/user-attachments/assets/18090802-0c94-4d51-9d3c-cc7ffb22c195">

For this exercise of configuring VXLAN, I will be using EVE-NG. EVE-NG is a network emulation tool that allows users to design, configure, and test network topologies in a virtual environment. I have installed EVE-NG bare metal on my server in my home. VXLAN-EVPN  is a technology used in modern data center networking to address challenges related to scalability, multi-tenancy, and mobility. VXLAN works with BGP primarily through the use of BGP EVPN  to manage the control plane for VXLAN overlay networks. Below is a topology of what i will be configuring:

<img width="1023" alt="Screenshot 2024-11-22 at 11 03 13 PM" src="https://github.com/user-attachments/assets/2755ff4c-1ab4-4e82-a807-6e6e7a46bdb8">


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

<img width="1152" alt="Screenshot 2024-11-22 at 11 04 05 PM" src="https://github.com/user-attachments/assets/8d8b993c-e7f8-4f25-99d3-1e13fea4e800">


Information about the system, CPU, threads, etc:

<img width="1507" alt="Screenshot 2024-11-22 at 11 05 09 PM" src="https://github.com/user-attachments/assets/7a47be14-f461-4b6b-b66a-879ff6f43fd4">


Information about the interfaces on the server:

<img width="1184" alt="Screenshot 2024-11-22 at 11 05 48 PM" src="https://github.com/user-attachments/assets/bc5af935-8dee-4ab9-a5b2-dfa8cf7a3b58">


Below is a screenshot on NAT that I have configured in order for EVE-NG to work properly:

<img width="782" alt="Screenshot 2024-11-22 at 11 06 15 PM" src="https://github.com/user-attachments/assets/ea5ad4ed-7967-4dd6-89d2-da3d2c1bdcb1">


I have several images installed including Cisco, Palo Alto, etc.:

<img width="900" alt="Screenshot 2024-11-22 at 11 07 36 PM" src="https://github.com/user-attachments/assets/3a3ce994-85e7-4efa-9d83-9a58d6687083">


<img width="844" alt="Screenshot 2024-11-22 at 11 08 09 PM" src="https://github.com/user-attachments/assets/5891b4f6-0aac-4a3d-aca0-164ce83c5696">


Once EVE-ng is installed and running, below if the main login page:

<img width="1506" alt="Screenshot 2024-11-22 at 11 08 35 PM" src="https://github.com/user-attachments/assets/f5e91fd7-75f3-4971-9178-d8a4a4101279">


Once logged in, you can see more details about the system itself:

<img width="1508" alt="Screenshot 2024-11-22 at 11 09 04 PM" src="https://github.com/user-attachments/assets/82e0066a-be34-40cc-b729-58d9b27b992f">


Including CPU: 

<img width="1506" alt="Screenshot 2024-11-22 at 11 09 31 PM" src="https://github.com/user-attachments/assets/646b1b61-44c7-490a-a164-959c4fcda0be">

You can also see full system details:

<img width="1504" alt="Screenshot 2024-11-22 at 11 10 03 PM" src="https://github.com/user-attachments/assets/d3d49de1-843c-408b-9711-876fd369d598">


### Summary

In this lab I was able to configure both VLAN-Aware and VLAN-Based approaches. Each configuration will be included below: 

+ [VLAN-Aware](https://github.com/lm3nitro/Projects/tree/main/VXLAN/VLAN-Aware)
+ [VLAN-Based](https://github.com/lm3nitro/Projects/tree/main/VXLAN/VLAN-Based)

