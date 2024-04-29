Atomic Red Team Install

1. Open PowerShell as Admin

2. To ignore the security warning prompts for the module:
```
powershell -ExecutionPolicy bypass
```
<img width="489" alt="Screenshot 2024-04-28 at 8 39 12 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/f3c8c74f-8630-42af-99c1-cc0bce281df9">

3. Install Execution Framework and Atomics Folder
```
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics
```
<img width="837" alt="Screenshot 2024-04-28 at 8 44 08 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/5961c236-6412-4aaa-b580-4b432a7c3fa2">

Don't mind the security warnings:

<img width="1348" alt="Screenshot 2024-04-28 at 8 44 45 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/35a9cb27-61e4-4927-87c2-9889224a09d8">

Once the installation is complete, you should see the following stating that it was successfully:
 
 <img width="859" alt="Screenshot 2024-04-28 at 8 45 08 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/46f9446c-e5a4-4d37-b752-cb2e7400d1c7">

We can also verify the everything was installed. By default, this gets stored in the C:\AtomicRedTeam:

<img width="782" alt="Screenshot 2024-04-28 at 8 50 11 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8c81dc14-e246-45aa-a154-2d0b19464bbd">

4.  import the module using the following command.
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force

update the PSDefaultParameterValues var
$PSDefaultParameterValues = @{"Invoke-AtomicTest:PathToAtomicsFolder"="C:\AtomicRedTeam\atomics"}

<img width="1218" alt="Screenshot 2024-04-28 at 9 01 54 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/e11ed5f8-7672-4aa1-890e-bd746e84ea3c">

5. Once that is imported, we can then check to see if it’s working by using the help command

<img width="833" alt="Screenshot 2024-04-28 at 9 07 19 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b9483a44-f1cf-45da-897d-3bae94deb60b">

6. See details of test with the following command:

Invoke-AtomicTest T1055 -ShowDetailsBrief

Here we see all the tests that matches the t1055
<img width="601" alt="Screenshot 2024-04-28 at 9 16 33 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/ab6ea997-e442-4d11-bb69-0ec9bd44fe29">

We will want to specify a test to see the exact details. in this case I chose T1055-5:
```
Invoke-AtomicTest T1055-5 -ShowDetails
```
<img width="955" alt="Screenshot 2024-04-28 at 9 20 55 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d2973b06-6348-40b5-85a7-c188fb4e18c2">

7. Check to see if we can run a particular test on the system:
```
Invoke-AtomicTest T1055-5 -CheckPrereqs
```
<img width="558" alt="Screenshot 2024-04-28 at 9 23 20 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/bd1d2bf3-6578-49f7-8690-831facaf21a7">

In this case, all the prereqs are met. If they are not met, you can use the following command:
```
Invoke-AtomicTest T1055-5 -GetPrereqs
```

8. Run the test
```
Invoke-AtomicTest T1055-5
```



