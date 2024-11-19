# Win 10 VM in ESXi

### Scope: 
This is a continuation of the previous installation, configuration, and patching of the ESXi. As a summary, upon installing the ESXi, I ran a vulnerability scan against the ESXi using Tenable and applied the patch to remediate what was found. Now that the ESXi server is fully patched, I can now get started wtih creating my VMs. Here I will go cover the process of creating a Win10 VM. 

### Tools and Technology:
VMWare ESXi and Windows

## Getting started

First, I needed to upload the Windows 10 VMDK files:

Select **Create/Register VM**

In the dialog box, select the following for the VM creation type:

![Pasted image 20240422194635](https://github.com/lm3nitro/Projects/assets/55665256/11482c39-5455-42ff-b519-f5dc306b34bc)

Under 'Select OVF and VMDK files' I am provided with the option to upload:
 
![Pasted image 20240422194739](https://github.com/lm3nitro/Projects/assets/55665256/a5d669f0-1eb5-40b3-aa30-66a43d202511)

Once I uploaded the VMDK file, I was presented with the Storage options:

![Pasted image 20240422194552](https://github.com/lm3nitro/Projects/assets/55665256/39e848b1-8fad-4be1-ab26-6027975d25bf)

Once I completed the steps and entered the desired parameters for the VM, I was able to see that it was successful:

![Pasted image 20240422194508](https://github.com/lm3nitro/Projects/assets/55665256/7dfdc0e6-ff0f-4329-a59c-8a4b84452e13)

I then powered on the VM to ensure everything was running smoothly:

![Pasted image 20240422194436](https://github.com/lm3nitro/Projects/assets/55665256/2a2dda2f-8bc3-4ef2-b5ab-2a49c3a1ccb0)

Here I can see that the VM is up and running. I also wanted to highlight the hardware settings and configuration that I used for this VM:

![Pasted image 20240422195542](https://github.com/lm3nitro/Projects/assets/55665256/8c78e5ce-4e1f-4924-a9e1-6af07253a874)

I also wanted to create a VM running Ubuntu so I followed the same steps as above, only this time using the Ubuntu files:

![Pasted image 20240422195220](https://github.com/lm3nitro/Projects/assets/55665256/8be826bb-71b9-4efe-9668-4efd8fba59be)

##  Summary:

I was able to successfully create both a Win10 and Ubuntu VM on my VMWare ESXi server. Its important to have several operating systems to test with. There are different applications and software that I will be using for my projects that require specific OS environments. Many organizations use a mix of both linux and Windows systems. Having both in my environment will allow me to see the interaction between applications running on both platforms. 
