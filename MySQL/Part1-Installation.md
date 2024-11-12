# MySQL
![Pasted image 20240504131247](https://github.com/lm3nitro/Projects/assets/55665256/d7430cd6-3c62-473e-832f-832762e18142)

MySQL is an open-source relational database management system. It allows us to store, manage, query and retrieve data stored in a relational database. 

### Scope: 
Here I will be installing MySQL on a Windows 10 host. Once installed I will run simple queries, intermediate queries, and will export findings. The queries are and exports will be done separately. These are the steps I used to get MySQL installed. 

### Tools: 
Windows 10, MySQL

### Installation:

To start, navigate to dev.mysql.com/downloads/installer/

I want to select the latest version. At the time of this writing it is 8.0.37. I also selected to download the MSI Installer that does not include the web as seen in the screenshot below:

![Pasted image 20240504111155](https://github.com/lm3nitro/Projects/assets/55665256/c5265906-9b8a-4858-ac15-81ebc876719c)

Once you select download, you will be redirected to login. At the bottom there is an option to bypass and continue without logging in:

![Pasted image 20240504111603](https://github.com/lm3nitro/Projects/assets/55665256/35b7f2dd-5aa5-4955-ac6f-8389e59db3fa)

Once it has been downloaded, double-click on the download and a new dialog box should appear:

![Pasted image 20240504112409](https://github.com/lm3nitro/Projects/assets/55665256/8820e5bd-70ca-401c-84f9-4e9787e2de18)

For setup type, I selected Full. Select *Next*

![Pasted image 20240504113023](https://github.com/lm3nitro/Projects/assets/55665256/617f4d72-9084-41c4-94ae-ebf8da0feddf)

In the next stage, you will see that it is ready to install under the status. To proceed, select *Execute*

![Pasted image 20240504113133](https://github.com/lm3nitro/Projects/assets/55665256/a20cbe33-dadb-4cf6-9b13-d1eded973803)

Once it has been completed, press *Next*

![Pasted image 20240504113531](https://github.com/lm3nitro/Projects/assets/55665256/e840e478-8d3e-4d5a-8263-e5e4e2d93558)

The next step would be to configure, press *Next*

![Pasted image 20240504113641](https://github.com/lm3nitro/Projects/assets/55665256/67142496-1ce3-4fa7-8d3d-95503aa869d0)

In the next step you can modify the port. I left it at the default port 3306:

![Pasted image 20240504113743](https://github.com/lm3nitro/Projects/assets/55665256/db7249cf-cc2e-4ccf-9b95-34f0359a7c76)

Select *Use Strong Password Encryption for Authentication* and select *Next*

![Pasted image 20240504113844](https://github.com/lm3nitro/Projects/assets/55665256/d4b092fe-7261-4c21-8516-4944d63bf0cf)

Here you can enter the password of your choosing. This will be needed later, so it's important to keep note of it:

![Pasted image 20240504114018](https://github.com/lm3nitro/Projects/assets/55665256/4686cb19-039f-4bca-8c7b-a64d5ce8cbbf)

Make sure that the following options are checked/selected

![Pasted image 20240504114114](https://github.com/lm3nitro/Projects/assets/55665256/8bd4c9a7-9fde-41bd-a74c-89cecbc317e8)

Grant the user running Windows Service full access

![Pasted image 20240504114203](https://github.com/lm3nitro/Projects/assets/55665256/191f02c0-6cc6-4437-831a-8eb944590b3c)

To apply the changes made to the configuration, select *Execute*

![Pasted image 20240504114540](https://github.com/lm3nitro/Projects/assets/55665256/93201a2f-5db2-4d6a-9308-7fd0eb3e0a89)

I was brought back to the configuration dialog window. However, since the first one (MySQL Server 8.0.37) was just configured I should see that the status reflects that change made. Next, I will configure *MySQL Router 8.0.37*

![Pasted image 20240504114644](https://github.com/lm3nitro/Projects/assets/55665256/c2bf5bdd-23e9-4bf4-8a9b-38f15c99e327)

This is mostly used with an InnoDB Cluster. I will not be using this, so nothing needs to be changed:

![Pasted image 20240504114740](https://github.com/lm3nitro/Projects/assets/55665256/81846311-9e86-40d1-8251-d9bcb1a60895)

Here I can see that the status for the *MySQL Router 8.0.37* has changed. Click *Next*

![Pasted image 20240504114832](https://github.com/lm3nitro/Projects/assets/55665256/46a847d1-72bb-4f14-a7ab-40d404c02fdf)

Here I will need to use the same password that I previously configured above. Click *Check*, once the check is complete, select *Next*

![Pasted image 20240504115033](https://github.com/lm3nitro/Projects/assets/55665256/81054583-0312-42a2-85e0-a5f76ec9ed50)

Here I will want to *Execute* and then *Finish*

![Pasted image 20240504115150](https://github.com/lm3nitro/Projects/assets/55665256/c28f47a8-bd27-44a5-b2fb-1c2829dad1f1)

I can also verify the installation logs; everything here looks good and at the bottom I see that it says it was successful:

![Pasted image 20240504115254](https://github.com/lm3nitro/Projects/assets/55665256/69f60592-4a10-4883-9052-6225ecf97712)

Everything has been complete, click *Next*
![Pasted image 20240504115352](https://github.com/lm3nitro/Projects/assets/55665256/b1a7b053-ff36-41b7-b83a-b0abbc48ce2d)

Select to start *MySQL Workbench* and *MySQL Shell* and select *Finish*
![Pasted image 20240504115626](https://github.com/lm3nitro/Projects/assets/55665256/7c94f9e7-f179-46d1-8115-9bfe45bedc62)

Once I select *Finish* above, MySQL Workbench and Shell will be opened. Now I can double-click and enter my password when prompted. 

![Pasted image 20240504120228](https://github.com/lm3nitro/Projects/assets/55665256/4dbad673-fb9b-4ffa-a3ac-0a0b4d8ade0b)

In continuation, I have created the following pages:

+ Import Databases.md
+ MySQL Queries.md
+ Connecting Remotely.md


