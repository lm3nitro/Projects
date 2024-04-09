# Sending Fortinet Logs to Splunk

Since integrating the new 60F Fortigate Firewall into my network, I wanted to ensure that I was also sending all my traffic logs to Splunk. Here are the steps taken and a look at the logs. 

First, I went to my Splunk instance and created a new Data Input:

<img width="1392" alt="Screenshot 2024-04-08 at 9 47 42 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/d1db6a81-cb53-49fd-94fe-7800e1ca11bd">

I then created a new UDP Data Input and set the port to 514 which is the default:

<img width="1398" alt="Screenshot 2024-04-07 at 11 00 48 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/8f258009-ab45-4795-aa2f-e3b315972ad8">

<img width="1352" alt="Screenshot 2024-04-07 at 11 01 14 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/bcb1b3d2-4cd1-48fb-8b11-5352afaa28a0">

In the next part, I went ahead and selected fortigate_log as the Source type, and selected the Fortinet fortigate Add-on for Splunk:

<img width="1217" alt="Screenshot 2024-04-07 at 11 03 08 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/c425da07-b06a-4790-bc5c-4e4a741c2438">

Within this same page, i also created a new index for these set of logs:

<img width="1425" alt="Screenshot 2024-04-07 at 11 03 58 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/d03941e9-6875-45ca-9b4c-64c1961a80f8">

Everything on the Splunk end is now complete:

<img width="1293" alt="Screenshot 2024-04-07 at 11 04 17 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/7c5872fa-629a-4746-8145-72aebd4d74dc">

Now, I headed over to my firewall. Once i logged in, I headed to 'Log & Report > Log Settings', set the Syslog logging to 'Enabled', Logging to 'All' and wrote the address to my Splunk instance:

<img width="996" alt="Screenshot 2024-04-07 at 11 05 55 AM" src="https://github.com/lm3nitro/Projects/assets/55665256/d9b956b2-d53a-425c-8c39-c96066013dda">
