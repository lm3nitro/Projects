# Installing VMWare ESXi:

VMware ESXi is a powerful hypervisor. It serves as the foundation for virtualization in data centers, allowing multiple virtual machines to run on a single physical server. It offers features such as live migration of VMs, high availability, resource allocation controls, and centralized management through tools like VMware vCenter Server.

I currently have a PowerEdge R720 server racked in my home networK. There are several VMs that I want to create for several mprojects that I have in mind, and will be installing VMWare ESXi on this server in order to get my virtual enviornmen tset-up. 

First thing I needed to do is go to VMWare to download ESXi:

![Pasted image 20240422100434](https://github.com/lm3nitro/Projects/assets/55665256/a5d6212b-56ef-49f8-b3da-e7d3dbbe34a8)

Once you login, go to **Downloads** 
![Pasted image 20240422100529](https://github.com/lm3nitro/Projects/assets/55665256/3926921c-c91a-478e-8394-5d9a2f051407)

Locate and select the product you are looking for, in my case it ESXi:

![Pasted image 20240422100714](https://github.com/lm3nitro/Projects/assets/55665256/12bc15ec-9b17-415d-a6f9-32b9dbf69b12)

Select **Download**

![Pasted image 20240422100135](https://github.com/lm3nitro/Projects/assets/55665256/afafe41d-f82d-44ab-bd74-174094ca2483)

This is the server that I have in my home and where I will be installing VMWare ESXI:

![Pasted image 20240422113930](https://github.com/lm3nitro/Projects/assets/55665256/0b043b5b-69f4-4512-ae1f-3c3cfc5b9811)

I saved the downloaded ISO to a USB and plugged it in my server, turned the server on, and went into the UEFI Boot Manager:

![Pasted image 20240422113823](https://github.com/lm3nitro/Projects/assets/55665256/4a1dee28-5214-40bf-b77d-a9e120e8d8b4)

Once there, go to the UEFI Boot Menu

![Pasted image 20240422113957](https://github.com/lm3nitro/Projects/assets/55665256/6a5fa65f-03c4-4944-9483-d288fc9fa784)

From there, select the USB that has the ISO:

![Pasted image 20240422114022](https://github.com/lm3nitro/Projects/assets/55665256/ad357273-a8da-4d88-b14b-acd96428b888)

Here we can see that it is loading the ESXi installer:

![Pasted image 20240422114108](https://github.com/lm3nitro/Projects/assets/55665256/a834f7c1-c387-44e4-b499-619db212e9b2)

Once that completes, I can see that the version we have is version 7 update 2:

![Pasted image 20240422114140](https://github.com/lm3nitro/Projects/assets/55665256/8b40b0d1-8487-4b04-ad1d-e56b89f3420f)

Click **(Enter) Continue**
![Pasted image 20240422114202](https://github.com/lm3nitro/Projects/assets/55665256/de363d8f-7b79-400a-ac69-18b89b45d466)

This is regarding the partitions, click **(Enter) Continue**

![Pasted image 20240422114306](https://github.com/lm3nitro/Projects/assets/55665256/239bbfb5-01a1-4e44-81ef-5102be4a0675)

Agree and Accept the Licensing terms:

![Pasted image 20240422114233](https://github.com/lm3nitro/Projects/assets/55665256/18616d03-1ef2-41c6-9611-3d2440896b97)

Confirm the selections and Install:

![Pasted image 20240422114346](https://github.com/lm3nitro/Projects/assets/55665256/f689a83a-847f-47b0-ac35-84b631102956)

We should thensee that it is being installed:

![Pasted image 20240422114428](https://github.com/lm3nitro/Projects/assets/55665256/20e41fc0-04a2-412b-b3f3-44ef0d5d4ab5)

After it has been installed, click to **Reboot**
![Pasted image 20240422114454](https://github.com/lm3nitro/Projects/assets/55665256/6d1b8fff-5f3e-445d-a2a4-a5b3b0c5ebf8)

When the server was restarting, I saw that it was trying to use DHCP to get an IP address. I did not want it to have a Dynamic IP address. To change this, select **Customize**:

![Pasted image 20240422114531](https://github.com/lm3nitro/Projects/assets/55665256/a378541f-0cb5-42e7-b03e-50bc1a4d2395)

Here we can set a static IP address:

![Pasted image 20240422114615](https://github.com/lm3nitro/Projects/assets/55665256/c8d72787-44b5-4435-b618-721a5a49af44)

Now we can see that the IP address has been applied:
![Pasted image 20240422114659](https://github.com/lm3nitro/Projects/assets/55665256/c0d02195-92cb-4f9f-a0dd-c9b60677c496)

![Pasted image 20240422114726](https://github.com/lm3nitro/Projects/assets/55665256/b88d52ae-a32e-4f9d-886d-2517a86ceef9)

Now that ESXi has ben instlalled and has a static IP address, I navigated to it and was present with the login page:

![Pasted image 20240422105501](https://github.com/lm3nitro/Projects/assets/55665256/f46e0bca-3b1c-48bd-a486-d3851c73d1e2)

These are the current settings, resources, and parameters that this ESXi instance has:

![Pasted image 20240422142450](https://github.com/lm3nitro/Projects/assets/55665256/05957018-ce07-4394-a809-abeb0f533660)

I then navigated to **Manage** > **Licensing** and applied my license
![Pasted image 20240422110717](https://github.com/lm3nitro/Projects/assets/55665256/7693efb3-7f79-40bc-9401-031a1ab32ab6)

We can then see that the license was applied:
![Pasted image 20240422110945](https://github.com/lm3nitro/Projects/assets/55665256/33db51c7-eb18-4190-9784-d26642a3827c)


