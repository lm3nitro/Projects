# Cisco Password Recovery

I will be performing a password reset on the Cisco 3750G switch. The **Cisco password recovery procedure** involves interrupting the switch’s normal boot procedure, renaming the **flash:config.text** (that’s the startup-config file for switches) to something else e.g **flash:config.text.old**, this way the configuration file is skipped during bootup.

Once the switch has loaded its operating system we can enter **privileged-exec mode**, rename back the **flash:config.text.old** to **flash:config.text** (**startup-config**), copy the **startup-config** file to memory (DRAM), make the necessary password changes and save the configuration.

Here is the process I followed:

## Password Recovery – Reset Procedure

The procedure described below assumes the **password recovery mechanism is enabled** (by default, it is) and there is physical access to the switch or stack (3750-X only).

>#### Note: If this procedure is being performed on a 3750-X stack, it is important to understand that all switches participating in the stack should be **powered off** and **only the Master switch is powered on** when initiating the password recovery procedure. The **Master switch** can be easily identified by searching for the switch with the green “Master” LED on.

**Step 1**

I am using a 3750-X switch, in this case I can **Power off** the entire stack or standalone switch. I then connected my console cable to the switch.

**Step 2**

Reconnect the power to the switch. Within 10 seconds, **press and hold** the **Mode button** while the **System LED** is **flashing green**. After the **System LED** turns **amber** and then **solid green**, **release** the **Mode button**.

If the process has been followed correctly, the following message should be displayed:

![Pasted image 20240415202632](https://github.com/lm3nitro/Projects/assets/55665256/78e553e3-4bbe-4e1f-9df2-893d3d30f09e)

Now **initialize** the flash file system, **rename** the **startup configuration** file (**config.text**) and **boot** the IOS:

![Pasted image 20240415202611](https://github.com/lm3nitro/Projects/assets/55665256/166ac53c-fad6-4ea7-a796-8c1304381cf7)

Now search for the **startup configuration** file (**config.text**) and rename it:

![Pasted image 20240415202917](https://github.com/lm3nitro/Projects/assets/55665256/ef3a47bb-e625-4cfb-baa3-e2efe9a0b56c)

We can now boot the switch IOS:

![Pasted image 20240415203004](https://github.com/lm3nitro/Projects/assets/55665256/67f0ae3d-ffac-4584-ab61-d310b31cbf0c)

![Pasted image 20240415203644](https://github.com/lm3nitro/Projects/assets/55665256/026520dc-0b14-4051-afc6-925a29ab43ee)

![Pasted image 20240415203532](https://github.com/lm3nitro/Projects/assets/55665256/1050b2d4-b35a-4699-a20d-0a4c974d9ef0)


At this point, the switch has booted bypassing its configuration file. At the prompt, type **enable** to enter **privileged exec mode** and **rename back** the **config.text.old** file:

![Pasted image 20240415204116](https://github.com/lm3nitro/Projects/assets/55665256/c0ca3428-2d30-4412-98cc-1599fb050704)


Finally, load the **startup configuration** of the master or standalone switch to memory and make the necessary changes to the **enable secret / password** or **user account** in question:
```
3750 (config) # **enable secret "password-here"
```
If you require to change the password to an account e.g admin, use the following command:
```
3750 (config) # **username admin privilege 15 secret "password-here"
3750 (config) # exit
```
Factory Reset :

![Pasted image 20240415211405](https://github.com/lm3nitro/Projects/assets/55665256/18218cab-edff-4562-9be4-a2a303f2149e)

Now that this has been completed, you can resume to re-configure the switch to meet your needs. 
