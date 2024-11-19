# Sending Sysmon Logs to Splunk

This is part 2 of the project. In the first part, I installed and configure Sysmon on the Win 10 OS host. The following will go through the process of sending the Sysmon logs to Splunk. I already have my Splunk instance up and running. 

## Splunk Universal Forwarder

To start, I will need to download the Splunk Forwarder on my Windows host. At the time of this writing, it is 9.2.1:

<img width="1124" alt="Screenshot 2024-04-29 at 10 34 34 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8efe8697-9e27-43aa-8cc7-90adaa53759d">

Right-Click on the Splunk Forwarder and select Install

<img width="1124" alt="Screenshot 2024-04-29 at 10 35 39 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/c36c8047-d9f6-4eb0-8dbc-1fe3ff2eeb9c">

I was presented with a new dialog box. I agreed to the license agreement and selected the type of Splunk instance I currently have deployed:

<img width="1024" alt="Screenshot 2024-04-29 at 10 37 22 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/5bfb65a7-2dd9-4eb4-ace7-5ee8a05e4136">

The installation location can be changed. I left the default location and selected *Next*

<img width="892" alt="Screenshot 2024-04-29 at 10 40 40 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/3617d921-34df-45bc-8adf-3e400910183f">

It also allows you to supply your own certificate. I went with the default:

<img width="856" alt="Screenshot 2024-04-29 at 10 41 10 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d641e79d-0860-40e3-be48-8c05dd399c81">

Next, I selected the data I wanted the SplunkForwarder to access. I chose local system:

<img width="871" alt="Screenshot 2024-04-29 at 10 41 56 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/51b323a7-2473-4698-a0a6-c13c1f274097">

I selected everything listed under Windows Event and Process Monitor. I will be modifying this later on:

<img width="885" alt="Screenshot 2024-04-29 at 10 42 57 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8e52f70a-2c9d-4d3e-b28c-9767a240ada8">

I then entered a username and password:

<img width="879" alt="Screenshot 2024-04-29 at 10 46 04 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a7a15fb3-5876-441e-8fcb-143d6e6ed0b3">

I will not be using a deployment server, so this is left intentionally blank:

<img width="882" alt="Screenshot 2024-04-29 at 10 53 09 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8cf0165f-2b77-40e3-bdb6-29468cd1ab9f">

Here I entered the information for my Splunk instance where the logs will be sent. By the default, the listening port is 9997:

<img width="866" alt="Screenshot 2024-04-29 at 10 54 43 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/ca4a66ac-4dca-4e58-b405-7f51824808c1">

This is now complete. It provides the option to go back and review the information entered to ensure everything is correct. Once complete, I click ed *Install*

<img width="865" alt="Screenshot 2024-04-29 at 10 55 13 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/6f5648ba-4b01-4627-9b25-21abe829ad8c">

<img width="866" alt="Screenshot 2024-04-29 at 10 56 09 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/61b7fccb-6046-42c3-88b2-7fdfdb1d892e">

Installation is finished, complete the install and click *Finish*
<img width="869" alt="Screenshot 2024-04-29 at 10 56 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/81aa0d9d-bd32-4bfa-b576-44607b72ad6c">

## Inputs.conf

When installing the Splunk Forwarder, In was able to choose what type of logs I wanted forwarded. These options all get placed in the *inputs.conf* file. By default, this is in the following location:

**C:\ProgramFiles\SplunkUniversalFowarder\etc\apps\SplunkUniversalFowarder\local\inputs.conf**

<img width="1125" alt="Screenshot 2024-04-30 at 12 22 55 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/e26ecdf3-5699-4588-874f-31fe7f54d162">

When opened using notepad, it looks like this:

<img width="633" alt="Screenshot 2024-04-30 at 9 26 32 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/03103ea2-a6ce-4c7c-a1ea-95ce0d9dc9cd">

I will be modifying this file in order to tailor the logs that I want to be sent to Splunk. To do this, I copied this file and placed the copied version of it in a different location. This is important because I want to ensure I have a backup when changing configuration settings. I renamed the copy/backup inputs.conf.bk:

<img width="642" alt="Screenshot 2024-04-30 at 12 25 06 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/b4ec6ff4-99eb-4b22-88ec-e4d966159f97">

I will be changing the contents of inputs.conf to the following example below. I have uploaded a copy of the inputs.conf file that I will be using. It can be found under *inputs.conf*:

<img width="1027" alt="Screenshot 2024-04-29 at 11 09 48 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b2e13f56-9bdb-4107-ba51-76c16a85ef11">

Now that the inputs.conf file is ready, I restarted the Splunk Forwarder. To do this, go to the *Start Menu* and type *Services:*

<img width="866" alt="Screenshot 2024-04-30 at 12 48 01 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/d324ec4a-30c4-42ae-bfb9-285c3f229aeb">

Locate the *SplunkForwarder* service and select restart. 

<img width="1018" alt="Screenshot 2024-04-30 at 12 48 58 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/865bfd97-dff1-42ca-99a4-0ed3bc644cee">

Now that the Splunk Forwarder is installed on the host, I needed configure my Splunk instance so that it can receive the logs being sent. 

## Splunk

First, I need to install 2 applications on my instance. These are both Splunk add-ons for Windows and Linux. For now, the Windows is the only one needed, however, I will be using the Linux one later. Once I downloaded the add-ons, I went to Splunk and navigated to *Upload from File*

<img width="1414" alt="Screenshot 2024-04-29 at 11 00 24 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b095b1e2-36a2-49f2-81f5-4f38b2ba23e5">

Choose the downloaded app/add-on:

<img width="1124" alt="Screenshot 2024-04-29 at 11 01 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8901557d-efb3-4a70-b1f9-14a499246cf3">

Select *Upload*

<img width="1046" alt="Screenshot 2024-04-29 at 11 02 18 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/021bf677-9a96-43fb-918f-ee107c836dd5">

Here I can see both files have been uploaded:

<img width="1430" alt="Screenshot 2024-04-30 at 12 30 07 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/9c2e5729-d67a-4ed2-b950-5bd87538c46f">

Once that was installed, I then needed to create an index. To do this go to *Settings > Indexes*

<img width="1417" alt="Screenshot 2024-04-29 at 11 25 19 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/233a1b8a-380b-49fa-b5e8-5c4305667148">

Click on *New Index*

<img width="1410" alt="Screenshot 2024-04-29 at 11 25 53 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8105e94e-ac68-4f03-b648-39608fc41bb6">

I named mine lm3nitro_pc

<img width="1116" alt="Screenshot 2024-04-29 at 11 26 37 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/78a4a323-9459-464d-9825-66074e0ce2b6">

Once the index was ready, I needed to set the receiving port in Splunk to 9997 to match the port I configured when installing the Splunk Fowarder above. As soon as the port was configured, I was able to see that I was receiving logs in my index:

<img width="1423" alt="Screenshot 2024-04-30 at 12 26 59 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/9b088a07-c283-4295-86d1-d414be9ab339">

## Verification

I then ran a couple of tests and verify that the logs are showing correctly. For these examples, I went back to my Windows host which has the Splunk Forwarder, opened a browser, and navigated to both *chess.com* and *lichess.org:*

<img width="1302" alt="Screenshot 2024-04-30 at 12 32 03 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/a315af3a-b3b3-4dbe-b6b0-64b267750e75">

<img width="1079" alt="Screenshot 2024-04-30 at 12 32 35 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/d0af7521-78a1-45d4-a4dc-12b02ac0e621">

I also can filter for these and see that this information is registering and being sent to Splunk:

<img width="1436" alt="Screenshot 2024-04-30 at 12 34 14 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/6adc7f65-f42a-49f1-acca-f0d476776fdb">

<img width="1435" alt="Screenshot 2024-04-30 at 12 36 22 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/f3181254-36f2-422f-929b-71fd71748e8e">

### Summary:

Having Sysmon configured and sending logs to Splunk provides real-time event logging of system activities which is important for monitoring, detecting, and responding to threats. By centralizing Sysmon logs in Splunk, it offers a range of benefits:

+ Analysis and alerting features to detect anomalies
+ Investigation of potential breaches
+ Comprehensive view of environment
+ This approach also helps improve threat detection, accelerates response times, and strengthens overall security posture


