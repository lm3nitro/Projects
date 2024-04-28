# Win 10 VM in ESXi

This is a continuation of the previous installation, configuration, and patching of the ESXi. As a summarym, upon installing the ESXi, I ran a vulnerability scan against the ESXi using Tenable and applied the patch to remediate what was found. Now that the ESXi server is fully patched, I can now get started wtih creating my VMs. To start, I will create a Windows 10 VM. 

### Uploading a Windows 10 VMDK files:

Select **Create/Register VM**
In the dialog box, select the following for the VM creation type:

![Pasted image 20240422194635](https://github.com/lm3nitro/Projects/assets/55665256/11482c39-5455-42ff-b519-f5dc306b34bc)

Under 'Select OVF and VMDK files' we are given the option to upload:
 
![Pasted image 20240422194739](https://github.com/lm3nitro/Projects/assets/55665256/a5d669f0-1eb5-40b3-aa30-66a43d202511)

Once we upload the VMDK file, we are then presented with the Storage options:

![Pasted image 20240422194552](https://github.com/lm3nitro/Projects/assets/55665256/39e848b1-8fad-4be1-ab26-6027975d25bf)

Once we complete the steps and have entered the desired parameters for the VM, we can see that it is successful:

![Pasted image 20240422194508](https://github.com/lm3nitro/Projects/assets/55665256/7dfdc0e6-ff0f-4329-a59c-8a4b84452e13)

We can now power on our VM and ensure everything is running smoothly:

![Pasted image 20240422194436](https://github.com/lm3nitro/Projects/assets/55665256/2a2dda2f-8bc3-4ef2-b5ab-2a49c3a1ccb0)

Here we can see that the VM is up and running. I also wanted to highligh the hardware settings and configuration that I used for this VM:

![Pasted image 20240422195542](https://github.com/lm3nitro/Projects/assets/55665256/8c78e5ce-4e1f-4924-a9e1-6af07253a874)

I also wanted to create a VM running Ubuntu so I followed the same steps as above, only this time using the Ubuntu files:

![Pasted image 20240422195220](https://github.com/lm3nitro/Projects/assets/55665256/8be826bb-71b9-4efe-9668-4efd8fba59be)

