# Sending Sysmon Logs to Splunk

The following will go through the process of sending the sysmon logs to Splunk. Currently, Sysmon is already installed on the host running Windows 10 and we have our Splunk instance up and running. 

## Splunk Universal Forwarder

To start, we will need to download the splunk Forwarder on our Windows host. At the time of this writing it is 9.2.1:

<img width="1124" alt="Screenshot 2024-04-29 at 10 34 34 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8efe8697-9e27-43aa-8cc7-90adaa53759d">

Right-Click on the Splunk Forwarder and select Install

<img width="1124" alt="Screenshot 2024-04-29 at 10 35 39 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/c36c8047-d9f6-4eb0-8dbc-1fe3ff2eeb9c">

You will be presented with a new dialog box. Agree to the license agreement and select the type of Splunk instance you have deployed

<img width="1024" alt="Screenshot 2024-04-29 at 10 37 22 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/5bfb65a7-2dd9-4eb4-ace7-5ee8a05e4136">

You can choose where you would like it installed. I left the default location and selected *Next*

<img width="892" alt="Screenshot 2024-04-29 at 10 40 40 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/3617d921-34df-45bc-8adf-3e400910183f">

If you wish to supply your own certificate, you can supply the needed details here. I went with the default:

<img width="856" alt="Screenshot 2024-04-29 at 10 41 10 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d641e79d-0860-40e3-be48-8c05dd399c81">

Next, select what data you would like the SplunkForwarder to access. I chose local system:

<img width="871" alt="Screenshot 2024-04-29 at 10 41 56 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/51b323a7-2473-4698-a0a6-c13c1f274097">

I selected everything listed under Windows Event and Process Monitor. I will be modifying this later on:

<img width="885" alt="Screenshot 2024-04-29 at 10 42 57 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8e52f70a-2c9d-4d3e-b28c-9767a240ada8">

Enter a username and password:

<img width="879" alt="Screenshot 2024-04-29 at 10 46 04 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a7a15fb3-5876-441e-8fcb-143d6e6ed0b3">

I will not be using a deployment server, so this is left intentionally blank:

<img width="882" alt="Screenshot 2024-04-29 at 10 53 09 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8cf0165f-2b77-40e3-bdb6-29468cd1ab9f">

Here you will need to enter the information for you Splunk instance where the logs will be sent. By the default, the listening port is 9997:

<img width="866" alt="Screenshot 2024-04-29 at 10 54 43 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/ca4a66ac-4dca-4e58-b405-7f51824808c1">

This is now complete. You can go back and review the information entered to ensure everything is correct. Once complete, *Install*

<img width="865" alt="Screenshot 2024-04-29 at 10 55 13 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/6f5648ba-4b01-4627-9b25-21abe829ad8c">

<img width="866" alt="Screenshot 2024-04-29 at 10 56 09 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/61b7fccb-6046-42c3-88b2-7fdfdb1d892e">

Installation is finished, complete the install and select *Finish*
<img width="869" alt="Screenshot 2024-04-29 at 10 56 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/81aa0d9d-bd32-4bfa-b576-44607b72ad6c">

## Splunk

Now that the Splunk Fowarder is installed on the host, we will need to configure our Splunk instance so that it can receive the logs being sent. 

<img width="1414" alt="Screenshot 2024-04-29 at 11 00 24 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b095b1e2-36a2-49f2-81f5-4f38b2ba23e5">

First, we will need to install 2 applications on our instance. These are both Splunk add-ons for Windows and Linux. For now the Windows is the only one needed, however, I will be using the Linux one later:

<img width="1124" alt="Screenshot 2024-04-29 at 11 01 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8901557d-efb3-4a70-b1f9-14a499246cf3">

<img width="1046" alt="Screenshot 2024-04-29 at 11 02 18 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/021bf677-9a96-43fb-918f-ee107c836dd5">

<img width="1416" alt="Screenshot 2024-04-29 at 11 04 04 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8357c9f7-7c17-4374-9c54-f1a16c23639c">

<img width="1027" alt="Screenshot 2024-04-29 at 11 09 48 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/b2e13f56-9bdb-4107-ba51-76c16a85ef11">

<img width="1417" alt="Screenshot 2024-04-29 at 11 25 19 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/233a1b8a-380b-49fa-b5e8-5c4305667148">

<img width="1410" alt="Screenshot 2024-04-29 at 11 25 53 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8105e94e-ac68-4f03-b648-39608fc41bb6">

<img width="1116" alt="Screenshot 2024-04-29 at 11 26 37 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/78a4a323-9459-464d-9825-66074e0ce2b6">

<img width="1413" alt="Screenshot 2024-04-29 at 11 27 40 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/7b051c43-70ea-4f10-a69c-27dbb8e611c9">

<img width="1188" alt="Screenshot 2024-04-29 at 11 29 10 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/bf521add-dcd3-4948-b3f2-d371ce5e1c2f">

<img width="1285" alt="Screenshot 2024-04-29 at 11 30 00 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/ee94adc1-c1a0-4419-9db0-e9b301f2ed52">

<img width="1125" alt="Screenshot 2024-04-30 at 12 22 55 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/e26ecdf3-5699-4588-874f-31fe7f54d162">

<img width="642" alt="Screenshot 2024-04-30 at 12 25 06 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/216461fd-85bf-476c-b4ff-973d88eebe7c">

<img width="1029" alt="Screenshot 2024-04-30 at 12 25 50 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/23c17a84-8749-451a-a479-5f663d6c23a5">

<img width="1423" alt="Screenshot 2024-04-30 at 12 26 59 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/9b088a07-c283-4295-86d1-d414be9ab339">

<img width="1421" alt="Screenshot 2024-04-30 at 12 28 38 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/3065ac0f-d274-4a5e-96a5-7a0f9921ef9b">

<img width="1430" alt="Screenshot 2024-04-30 at 12 30 07 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/ba9a93f6-b3c6-4998-b758-a4b6272fa236">

<img width="1302" alt="Screenshot 2024-04-30 at 12 32 03 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/a315af3a-b3b3-4dbe-b6b0-64b267750e75">

<img width="1079" alt="Screenshot 2024-04-30 at 12 32 35 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/d0af7521-78a1-45d4-a4dc-12b02ac0e621">

<img width="1436" alt="Screenshot 2024-04-30 at 12 34 14 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/6adc7f65-f42a-49f1-acca-f0d476776fdb">

<img width="1435" alt="Screenshot 2024-04-30 at 12 36 22 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/f3181254-36f2-422f-929b-71fd71748e8e">

<img width="866" alt="Screenshot 2024-04-30 at 12 48 01 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/d324ec4a-30c4-42ae-bfb9-285c3f229aeb">

<img width="1018" alt="Screenshot 2024-04-30 at 12 48 58 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/865bfd97-dff1-42ca-99a4-0ed3bc644cee">
