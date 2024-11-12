# $${\color{red}Atomic \space Red \space Team \space on \space Win \space OS}$$ 

The Atomic Red Team is a framework that is used to perform security testing and threat emulation. This allows organizations the ability to test their capability to detect and respond to real world threats. Each test offered within the Atmoic Red Team is used to simulate a specific attack and technique. 

### Scope: 

I will be using a Windows 10 OS that has a Wazuh agent already installed on it. I will be installing the Atomic Red Team on the host and performing a series of test and anlyzing the detection in Wazuh.   

### Tools/Technology: 

Atomic Red Team, Win OS, PowerShell, and Wazuh

<img width="487" alt="Screenshot 2024-04-28 at 9 53 43 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/4abf46bc-28ec-4d72-b209-a0975ed26350">

## Install/Import Atomic Red Team

1. To get started, I opened PowerShell as Admin on the Windows host. 

2. To ignore the security warning prompts for the module, I used the following command:
   
```
powershell -ExecutionPolicy bypass
```

<img width="489" alt="Screenshot 2024-04-28 at 8 39 12 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/f3c8c74f-8630-42af-99c1-cc0bce281df9">

3. Next, I installed the Execution Framework and Atomics Folder. The Atomic folder will contain the test definitions that are needed in order to test. I used the following command:
   
```
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics
```

<img width="837" alt="Screenshot 2024-04-28 at 8 44 08 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/5961c236-6412-4aaa-b580-4b432a7c3fa2">

After running the commands, I got some security warnings:

<img width="1348" alt="Screenshot 2024-04-28 at 8 44 45 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/35a9cb27-61e4-4927-87c2-9889224a09d8">

Once the installation completed, I was presented with the following output showing that it had completed successfully:
 
<img width="859" alt="Screenshot 2024-04-28 at 8 45 08 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/46f9446c-e5a4-4d37-b752-cb2e7400d1c7">

I also verified that everything was installed. By default, this gets stored in the C:\AtomicRedTeam:

<img width="782" alt="Screenshot 2024-04-28 at 8 50 11 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8c81dc14-e246-45aa-a154-2d0b19464bbd">

4. Now that I got that installed, I imported the module using the following command:

```
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
```

```
update the PSDefaultParameterValues var
$PSDefaultParameterValues = @{"Invoke-AtomicTest:PathToAtomicsFolder"="C:\AtomicRedTeam\atomics"}
```

<img width="1218" alt="Screenshot 2024-04-28 at 9 01 54 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/e11ed5f8-7672-4aa1-890e-bd746e84ea3c">

5. Once that was imported, I then checked to see if it was working by using the help command:

```
help Invoke-AtomicTest
```

<img width="833" alt="Screenshot 2024-04-28 at 9 07 19 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b9483a44-f1cf-45da-897d-3bae94deb60b">

## Testing 

6. There are a series of tests that are available. To see the details of a test I used the following command. Here I am taking a look at the test T1055:
   
```
Invoke-AtomicTest T1055 -ShowDetailsBrief
```

There are a series of test that start with T1055, the output shows all the matches:

<img width="601" alt="Screenshot 2024-04-28 at 9 16 33 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/ab6ea997-e442-4d11-bb69-0ec9bd44fe29">

In order to see the exact details of a test, it needs to be specified. In this case I chose T1055-5:

```
Invoke-AtomicTest T1055-5 -ShowDetails
```

<img width="955" alt="Screenshot 2024-04-28 at 9 20 55 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d2973b06-6348-40b5-85a7-c188fb4e18c2">

7. There are a some tests that will require certain pre-requisites in order for the test to be executed properly. To check to see if I am able to run a particular test on the system, I used the following command:
   
```
Invoke-AtomicTest T1055-5 -CheckPrereqs
```

<img width="558" alt="Screenshot 2024-04-28 at 9 23 20 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/bd1d2bf3-6578-49f7-8690-831facaf21a7">

In this case, all the pre-requisites are met. In the case where the pre-requisites are not met, you can use the following command:

```
Invoke-AtomicTest T1055-5 -GetPrereqs
```

8. For my test, I decided to run the following test (T1564.002-3). This test creates a hidden user in registry.

```
Invoke-AtomicTest T1564.002-3
```

<img width="611" alt="Screenshot 2024-04-28 at 9 36 24 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/1cba1d7a-d1eb-4761-af99-d9daf02ffc11">

I was able to see that the test completed successfully.

## Analysis

9. I then verified the registry key to ensure that the test did create the hidden user:

<img width="1034" alt="Screenshot 2024-04-28 at 6 11 57 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/cf42886c-1484-4b96-b9b7-1629c53656c6">

This host does have a Wazuh agent installed. I wanted to see the details that the agent was able to capture for this test. 

<img width="1427" alt="Screenshot 2024-04-28 at 9 38 41 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b31f6cea-d459-4788-a468-3b2d52802f05">

When I expand and see the details, I can see that this event was generated from the test:

<img width="1396" alt="Screenshot 2024-04-28 at 9 39 14 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/c20b8d6e-c688-4581-95db-b723dcbc43a2">

## Summary and Analysis: 

I continued to run a series of other test and analyzed the logs provided by Wazuh. Although it is always best practice to have a series of security measures and not just one, I could see that Wazuh was able to capture all the security events I tested against the host. For being an open source tool, I beleive that Wazuh provided great insight and details about the test and the triggers. Overall, Atomic Red Team provides a way to simulate a series of different attacks in a controlled environment. The Atomic Red Team helps assess the effectiveness of current security measures, identify gaps, and improve incident and detection response capabilities. Using the Atomic Red Team also allowed me to facilitate hands-on learning and understanding of attack methods. 
